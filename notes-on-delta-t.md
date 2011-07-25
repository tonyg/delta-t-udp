# Tony's Notes on Delta-t

MPL = max packet lifetime  
R = max time sender willing to keep retransmitting  
A = max ack delay  
T = max new SN generation rate  
SNs range within `[0,2^n)`

    2^n > (2*MPL + R + A)T

Let MPL = 120s, R = 900s, A = 60s, T = 100 gigabits per second. Then
we need a power of two greater than 1.2 x 10^14 bits. This is beyond
the capability of a 32-bit counter, but a 64-bit counter will do
nicely.

Delta-t is duplex, but should probably be simplex.

Rule R.2 is a bit weird. What's going on there?

What's "data overflow" in R.3? Ah, it's arrival of data outside our
current receive window. Flow-control related.

The Pdrf flag is interesting, letting out-of-order packets be detected
when a fresh state vector is needed!

The flow control section is interesting because it makes it clear that
in 1981 TCP's flow-control hadn't been well developed. I wonder how
much work it'd be to lift TCP's flow-control mechanism across to a
Delta-t setting?

Congestion control would be the other piece missing from Delta-t.

I don't like the closing of the window on transmission of an "E"
bit. Makes pipelining messages rather tricky! (Section on Window
Management.)

Wrt "Rendezvous" and "reliable-Ack": isn't there a vulnerability
there, where a malicious Rendezvous could trigger unbounded
retransmission of reliable-Acks? Also since you have to remember that
a rendezvous was requested, isn't it impossible to remove the
rendezvous bit? It seems to become hard state.

It really looks like there are hard-stateish races around the
rendezvous packets.

Section 3.1, "ports" are 64-bit addresses.
