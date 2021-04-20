# orangesegments~
## a Pure Data abstraction, modular arithmetic sound file divider, orangesegments~.pd

So here's the concept:

To rotate through an audio file, a patch that picks out little pieces, searching for resonant math, the melodies and rhythms that crop up. 

The name comes from "The Orange Garden" which was a project for Cities and Memory. For my contribution, I explored the pattern-recognition of a human listener in a park, the fact that certain sounds have regularities, periods. The birds call every few seconds, a person walks by every few half-minutes, a plane passes every couple minutes… Music has those kinds of periods too. By dividing a field recording of a park into divisions of equal size, sequential playback locks all the clips to a rhythm and the sounds inside become easier to hear as a melody. 

That patch was based on a random shuffle of the divisions, but orange segments is based on modular arithmetic - because each of the divisions are assigned a number (an integer, an address), they become sortable. By jumping ahead a set number of segments each step in the sequence, the output can be quickly locked to a loop, set to advance linearly a little bit each time it cycles back, lots of different kinds of reordering... And both the amount of divisions and the amount of segments advanced each step can be changed on the fly. 


The patch: 

The first element ("orange") loads a wav file into an array. The second ("segments") marks a variable amount of equidistant points within that array. The third ("rotate") adds an integer through a modulo operation that decides how many segments to skip each step. Each time a segment finishes playing, it jumps to the next address. There's also a button to jump ahead, instead of having to wait for a long segment to finish. 

There are two signal outlets for stereo output, left and right. The second two outlets are useful for time syncing. The left outlet outputs a bang the moment before a new segment plays, and the right outlet outputs the segment length in milliseconds. 

<img src="https://github.com/nesasmr/orangesegments/blob/main/patchpic.png">


The analogy:

Imagine a peeled orange rolling down a sandy dune.

If the dune isn't very steep, the orange will roll slowly down the hill. It will leave a mark in the sand that looks like an uninterrupted line, and all the segments will get equally sandy. This is like playing the audio file 1 segment at a time, and would sound like your average audio playback (plus the little fades at each segment start and end). 

But if the dune is steep, the orange will roll and bump and bounce down the hill, leaving a series of orange-shaped indentations in the sand. Changing the rotation amount changes how much the orange spins in the air before it hits the ground again. 

If the orange rotates one complete turn each time it hits the sand, the same segment gets sandy every time the orange hits the ground. 

Oranges have 10 segments (which is a good figure to know, because it's time to do a little math). If the orange rotates 5 segments each time it bounces, it will rotate one half turn through its 10 segments, and only the first and the segment opposite the first will ever touch the dune. This is a two-segment loop.

If it rotates 3 segments each time it bounces, the first segment will get sandy, then the fourth, then the seventh, then the tenth, then the third, then the sixth, then the ninth, then the second, then the fifth, then the eighth, then the first again, then the fourth again, and on and on. In this situation, all the segments get sandy, even though they don't get sandy in order. 


The math: 

Modular arithmetic shows up in analog clocks - it always frustrated me as a kid that 3 hours after 10pm is 1am, not 14pm...
The day is divided into 24 hours, like the orange is divided into 10 segments. I have an alarm that wakes me up at 8am every morning - that alarm is set to go off once every 24 hours. You might set an alarm to go off every 12 hours - something like 8am and 8pm, or 6am and 6pm. If you set an alarm to go off once every 8 hours, it will ring 3 times every day - something like 6am and 2pm and 10pm, then again at 6am the next morning. 

These are a few examples of resonant frequencies - because 24 can be divided simply into 2 (a 12 hour alarm) and 3 (an 8 hour alarm), the alarms will land at the same hour one day to the next. 24 doesn't divide simply into groups of 23, though, so if you were to set an alarm to go off every 23 hours, it might ring at 8am on Monday, but by the time Tuesday rolled around, it would ring at 7am. 

Being able to recognize simple fractions between the amount-of-segments and the segments-per-rotation is musically useful, because getting close to those numbers resembles the sound of a looping rhythm, even though you're not returning to the exact same segment each time. For example, an audio file divided into 100 segments and rotated every 51 segments will alternate between the beginning and the middle of the file, but every time it rotates back, it moves forward a segment. If that audio file with 100 segments is instead rotated every 33 segments, it will sound very much like a waltz, a loop bouncing between three evenly spaced segments, but every time you hear the waltz, it changes a little bit, making up for the fact that while 33+33+33 is almost 100, it's not quite. 

The amount-of-segments divided by the segments-per-rotation can equal number-of-beats-per-loop, as long as that number simplifies to a whole integer and the number of segments-per-rotation is less than the amount-of-segments. Otherwise, it can still give clues to perceived rhythm (2.1 and 0.49 will probably sound duple as they are close to 2 or 1/2; 2.9 and .333 will probably sound triple as they are close to 3 or 1/3).

Taking advantage of the modular arithmetic can lead to some really beautiful complex rhythms. Loops with a large number of beats (think 9, 15, 21, etc) are certainly possible without doing the modular arithmetic calculations (a loop of 15 beats is possible by dividing an audio file into 150 segments and rotating every 10 segments, essentially 150/100=15). Kind of like stop motion animation, because those segments will all be played back in the same order as they were originally in the audio file, preserving the same linear time quality of an original recording. Here, 15 is made by counting from 1 to 15. 
But 15 can also be made from 2+3+3+2+2+3, or 3+4+8, or 10+5, or 6+9, or 8+7... I'm thinking of cribbage. By using numbers that don't divide down to a whole number (or the inverse of a whole number when segments-per-rotation is greater than amount-of-segments), but do share a common multiple, certain sections will reappear in the loop. If an audio file starts with water, has music in the middle, and ends with a train passing by, a linear/whole number division into 15 might sound like 5 beats of water, 5 beats of music, 5 beats of train. But by overshooting the amount of segments, by rotating fully multiple times before closing the loop, those sections can be shuffled - something like 2 beats water, 3 beats train, 1 beat music, 2 beats train, 3 beats water, 4 beats music. In this example, the 1 beat music is linked in the mind to the 4 beats music, and a more complex rhythm is created. 

100 segments rotated 20 is a 5 beat loop, because 100/20=5. 
100 segments rotated 80 is a 5 beat loop, because 100/80=5/4.

100 segments rotated 10 is a 10 beat loop, because 100/10=10. 
100 segments rotated 70 is a 10 beat loop, because 100/70=10/7.

100 segments rotated 5 is a 20 beat loop, because 100/5=20. 
100 segments rotated 45 is a 20 beat loop, because 100/45=20/9.


To be honest:

I'm not really doing the math in my head. Because the amount of divisions decides the length of the segments, tempo is dependent on both the length of the original audio file and the amount of segments that file is divided into - it's not always going to be 100. So generally I try to trust my gut. I'm not a mathematician, but even if I did have a super useful formula to get musical loops from divisions of 78 or 310 or 570... I don't know if I would. It feels like really good luck when you find a cool loop. 

And that's the point! 


## For audio examples
You'll find an album at https://nicoloscolieri.bandcamp.com/album/orangesegments - a gathering of the little recordings I made while I was developing the patch. These examples were originally - a recording of me playing flute at home, a couple field recordings of neighborhood sounds, flute I had already processed with a delay pedal, an ambient synth track made with Ableton's Operator device, and a recording from the back of a rickety van. They were recorded within Pure Data, then excerpted and normalized in Ableton. 

## Installation
To run this software, you'll need to have Pure Data installed. PD can be downloaded at https://puredata.info/downloads/pure-data
