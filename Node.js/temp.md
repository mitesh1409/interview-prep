
Studying...  
The Node.js Event Loop  
https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick

NOTE: I have provided contents of a section in which I have doubts.  

Doubts/Queries  

## #1  
<content>
Each phase has a FIFO queue of callbacks to execute.  

While each phase is special in its own way, generally, when the event loop enters a given phase, it will perform any operations specific to that phase, then execute callbacks in that phase's queue until the queue has been exhausted or the maximum number of callbacks has executed.  

<query>
Query #1  
when the event loop enters a given phase, it will perform any operations specific to that phase  
What does this mean?

Answer  
Each phase isn't *just* a queue of callbacks. Some phases have internal bookkeeping or system-level work they do *before* draining their callback queue.

For example, the **timers** phase doesn't just run callbacks — it first checks the system clock to determine which `setTimeout`/`setInterval` thresholds have been crossed, *then* runs those callbacks. The **poll** phase calculates how long it can afford to block waiting for I/O events before it needs to move on. These internal operations are what "operations specific to that phase" refers to.
</query>

When the queue has been exhausted or the callback limit is reached, the event loop will move to the next phase, and so on.  
</content>

---

## #2  
<content>
Phases Overview  

timers: this phase executes callbacks scheduled by setTimeout() and setInterval().

pending callbacks: executes I/O callbacks deferred to the next loop iteration.

<query>
Query #2  
When and how I/O callbacks are deferred?  
My guess is - each phase's queue has a set limit of maximum number of callbacks that can be executed by the Event Loop per iteration. So remaining callbacks are deferred to be executed in the next iteration.  
I am not sure about this, just guessing, please correct me if I am wrong.

Answer  
When an I/O operation completes, its callback is normally executed in the **poll** phase. However, in certain cases — primarily when the OS reports an error for some operations (like a TCP connection being refused, `ECONNREFUSED`) — the callback cannot be cleanly handled in the poll phase during that iteration. So Node.js defers it to the **pending callbacks** phase of the *next* iteration instead.

That's really the main scenario the "pending callbacks" phase exists for. It's a narrow, specific deferral — not a general-purpose overflow mechanism.
</query>

idle, prepare: only used internally.

poll: retrieve new I/O events; execute I/O related callbacks (almost all with the exception of close callbacks, the ones scheduled by timers, and setImmediate()); node will block here when appropriate.

<query>
Query #3  
retrieve new I/O events  
What does this mean?  

Answer  
When your Node.js program does something like reading a file or receiving network data, the underlying OS handles the actual work. The **poll** phase asks the OS: *"Hey, are any I/O operations done? Give me the events."* This is done via OS-level system calls like `epoll` (Linux), `kqueue` (macOS), or `IOCP` (Windows). "Retrieving new I/O events" means calling into the OS to collect the results of completed I/O operations, and then running the callbacks associated with them.
</query>

<query>
Query #4  
node will block here when appropriate  
What does this mean?  

Answer  
This is one of the more important and subtle points. When the poll queue is empty and there's nothing scheduled (no `setImmediate`, no expired timers), the event loop doesn't spin furiously doing nothing — that would waste CPU. Instead, it **blocks** (sleeps) inside the OS poll call, telling the OS: *"Wake me up when an I/O event arrives or when a timer is about to expire."*
</query>

check: setImmediate() callbacks are invoked here.

close callbacks: some close callbacks, e.g. socket.on('close', ...).
</content>

---

## #3  
<content>
Starting with libuv 1.45.0 (Node.js 20), the event loop behavior changed to run timers only after the poll phase, instead of both before and after as in earlier versions. This change can affect the timing of setImmediate() callbacks and how they interact with timers in certain scenarios.
</content>

<query>
Query #5  
Order of execution of Event Loop phases before libuv 1.45.0 (Node.js 20):  
1 - timers
2 - pending callbacks
3 - idle, prepare
4 - poll
5 - check
6 - close callbacks

Order of execution of Event Loop phases starting with libuv 1.45.0 (Node.js 20):  
1 - pending callbacks
2 - idle, prepare
3 - poll
4 - timers
5 - check
6 - close callbacks

Is the above correct?

Answer  
*Confusing  
Your order for the **old** behavior is correct. For the **new** behavior (libuv ≥ 1.45.0 / Node.js 20+), your order is mostly right in spirit, but the actual phase sequence itself doesn't change — timers is still technically the "first" phase in the loop structure. What changed is more subtle: **timers are only evaluated after the poll phase completes**, rather than also being checked at the start of the loop. So the phases are still listed in the same order in documentation, but *when timers fire relative to poll* changed. In practice for your ordering, yes — timers now effectively execute after poll, which means `setImmediate` (check phase, right after poll) can fire *before* a ready timer in some scenarios where previously the timer might have fired first.
</query>

---

## #4
<content>
poll  
The poll phase has two main functions:

1. Calculating how long it should block and poll for I/O, then
2. Processing events in the poll queue.

<query>
Query #5  
1. Calculating how long it should block and poll for I/O, then
What does this mean?

Answer  
The event loop calculates a *timeout* — the maximum time it's willing to wait. This is determined by the soonest scheduled timer. If a `setTimeout(fn, 100)` is pending, the poll phase won't block for more than ~100ms, so it can wake up in time to run that timer. If no timers are scheduled, it may block indefinitely (until I/O arrives).

2. Processing events in the poll queue.
What does this mean? There are events and linked callbacks, so it will take care of both of them, right?

Answer  
Yes, events and their linked callbacks are processed together. An "event" here means a completed I/O operation; the poll phase retrieves the event from the OS and immediately invokes the associated callback.
</query>

When the event loop enters the poll phase and there are no timers scheduled, one of two things will happen:

* If the poll queue is not empty, the event loop will iterate through its queue of callbacks executing them synchronously until either the queue has been exhausted, or the system-dependent hard limit is reached.

<query>
Query #6  
When system-dependent hard limit is reached, what will happen to rest of the callbacks?
I guess - Event Loop moves to the next phase and then the rest of these callbacks will be executed in the next iteration of the Event Loop, right?

Answer  
Yes, your guess is correct. The remaining callbacks stay in the queue and the event loop moves on to the next phase (check → close callbacks → back to timers). Those remaining callbacks will be picked up in the **next iteration's poll phase**.
</query>

* If the poll queue is empty, one of two more things will happen:

    * If scripts have been scheduled by setImmediate(), the event loop will end the poll phase and continue to the check phase to execute those scheduled scripts.

    * If scripts have not been scheduled by setImmediate(), the event loop will wait for callbacks to be added to the queue, then execute them immediately.

Once the poll queue is empty the event loop will check for timers whose time thresholds have been reached. If one or more timers are ready, the event loop will wrap back to the timers phase to execute those timers' callbacks.
</content>

<query>
Query #7  
How the Event Loop will process callbacks in the following scenario:  
One callback scheduled using `setTimeout`, timer value = 100ms  
One callback for a file I/O, assuming file I/O takes 110ms  
One callback scheduled using `setImmediate`  

As per my understanding order of execution will be:  
#1 Synchronous code executes, Call Stack becomes empty
#2 While executing Synchronous code, Event Loop registers callbacks for `setTimeout`, file I/O & `setImmediate` whenever it encounters Asynchronous code, does not block the main thread. Event Loop then executes asynchronous tasks - timer & file I/O using Thread Pool or OS Kernel, once they are completed it pushes linked callbacks into Callback Queue. Event Loop executes all of this in the background while Synchronous code is executing.
#3 Order of Event loop phases - timers -> pending callbacks -> idle, prepare -> poll -> check -> close callbacks.

It starts with timers phase but no timers expired yet.
pending callbacks & idle, prepare phases used internally by Node.js.
Then comes poll phase, file I/O still in progress, no callbacks yet.
Then in check phase we have one callback for `setImmediate` so it will be picked up by Event Loop for execution.
Nothing in close callbacks phase.
First iteration is over.

In the next iteration,
Timer expires (100ms) before file I/O (110ms) so timer callback will be executed first.
Then file I/O will be picked up.

So order of execution is:  
#1 `setImmediate` callback
#2 `setTimeout` callback (because timer expires before file I/O completes)
#3 file I/O callback
</query>

---

## #5
<content>
</content>

