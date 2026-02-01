# Strudel Cookbook (Curated Examples)

This file is a curated grab-bag of examples that ship with this skill.
All code blocks are included verbatim for convenience.

Use it when you want a known-good idiom to adapt.

## Mini-Notation Essentials

```javascript
sound("bd bd sd hh bd cp sd hh")
```

```javascript
sound("metal ~ jazz jazz:1")
```

```javascript
sound("bd wind [metal jazz] hh")
```

```javascript
sound("bd*2, hh*2 [hh oh]")
```

```javascript
note("[c a f e]/2")
```

## Euclidean / Polymeter

```javascript
s("bd(3,8)")    // 3 beats distributed over 8 steps
s("hh(5,8,2)")  // 5 beats over 8 steps, offset by 2
s("cp(7,16)")   // 7 beats over 16 steps
```

```javascript
s("<bd rim, hh hh oh>*4")  // Different pattern lengths cycling
```

## Pattern Transforms (Good for Live Iteration)

```javascript
n("0 1 [4 3] 2 0 2 [~ 3] 4").sound("jazz").rev()
```

```javascript
n("0 1 [4 3] 2 0 2 [~ 3] 4").sound("jazz").jux(rev)
```

```javascript
// Simple addition
note("c2 [eb3,g3]".add("<0 <1 -1>>"))
.sound("gm_acoustic_bass")

// With scales
n("0 [2 4] <3 5> [~ <4 1>]".add("<0 [0,2,4]>"))
.scale("C5:minor")
```

```javascript
sound("hh hh, bd rim [~ cp] rim").ply(2)
// Same as: sound("hh*2 hh*2, bd*2 rim*2 [~ cp*2] rim*2")
```

```javascript
n("0 [4 <3 2>] <2 3> [~ 1]")
.off(1/16, x=>x.add(4))
.scale("C5:minor")
```

```javascript
s("bd sd").echo(3, 1/6, .8)
// times, offset time, feedback
```

## Time Control

```javascript
setcpm(45)  // Set global tempo
sound("bd sd [~ bd] sd")
```

```javascript
sound("bd sd [~ bd] sd").fast(2)    // Double speed
sound("bd sd [~ bd] sd").slow(2)    // Half speed

// Pattern tempo changes
note("c2, eb3 g3 [bb3 c4]").sound("piano").slow("0.5,1,1.5")
```

## Harmony: Scales / Chords / Voicings

```javascript
// Scale degrees (0-indexed)
n("0 2 4 6 4 2").scale("C:major")

// Pattern scales
n("[0,7] 4 [2,7] 4")
.scale("C:<major minor>/2")

// Random notes in scale
n(rand.range(0,12).segment(8))
.scale("C:ritusen")
```

```javascript
// Create bass line from chord progression
"<C^7 A7b13 Dm7 G7>*2".rootNotes(3).note()

// Combined with voicings for complete arrangement
"<C^7 A7b13 Dm7 G7>*2".layer(
  x=>x.voicings('lefthand').struct("[~ x]*2").note(),  // Chords
  x=>x.rootNotes(2).note().s('sawtooth').cutoff(800)   // Bass
)
```

## FX: A Few Reliable Building Blocks

```javascript
// Important: Effects are single-use per pattern. You cannot chain multiple instances of the same effect (e.g., `.lpf(100).distort(2).lpf(800)` won't work as expected).
```

```javascript
n("0 3 7 5".slow(2))
.scale("A:minor")
.s("sawtooth")
.lpf(sine.range(200, 2000).slow(4))
.lpq(10)
.lpenv(3)
.shape(0.3)
```

```javascript
stack(
  n("0 2 4 6").s("sine").attack(2).release(3),
  n("0 2 4 6".add(7)).s("sine").attack(2.5).release(3.5).gain(0.7)
)
.scale("C:major")
.room(0.8).roomsize(5)
.lpf(2000)
```

```javascript
n("0 2 4 7 5 4 2 0")
.scale("E:minor")
.s("triangle")
.decay(0.3).sustain(0)
.lpf(1200).lpenv(2)
.room(0.3)
```

## Sampling / Loop Manipulation

## Loading External Sample Packs (Allowed)

### From GitHub (Sample Pack Shortcut)

```javascript
samples('github:tidalcycles/dirt-samples')
s("bd sd bd sd,hh*16")

// With specific branch
samples('github:user/repo/branch')
```

### From a Direct URL (strudel.json)

```javascript
samples('https://raw.githubusercontent.com/tidalcycles/Dirt-Samples/master/strudel.json')
s("bd sd bd sd,hh*16")
```

### From Direct URLs (Custom Mapping)

```javascript
samples({
  bassdrum: 'bd/BT0AADA.wav',
  hihat: 'hh27/000_hh27closedhh.wav',
  snaredrum: ['sd/rytm-01-classic.wav', 'sd/rytm-00-hard.wav'], // Multiple files
}, 'https://raw.githubusercontent.com/tidalcycles/Dirt-Samples/master/');

s("bassdrum snaredrum:0 bassdrum snaredrum:1, hihat*16")
```

### Local Sampler Server

```javascript
samples('http://localhost:5432/');
n("<0 1 2>").s("swoop smash")
```

```javascript
// Skip first quarter of sample
s("rave").begin("<0 .25 .5 .75>")

// Cut off end of sample
s("bd*2,oh*4").end("<.1 .2 .5 1>")
```

```javascript
s("bd*6").speed("1 2 4 1 -2 -4")  // Negative = reverse
speed("1 1.5*2 [2 1.1]").s("piano").clip(1)
```

```javascript
s("rhodes")
.chop(4)      // Cut into 4 parts
.rev()        // Reverse order of chops
.loopAt(2)    // Fit into 2 cycles
```

```javascript
// Cut into 8 slices, trigger with pattern
s("breaks165").slice(8,"0 1 <2 2*2> 3 [4 0] 5 6 7".every(3,rev))

// Slice at specific points
s("breaks125").fit().slice([0,.25,.5,.75],"0 1 1 <2 3>")
```

```javascript
s("breaks165")
.splice(8, "0 1 [2 3 0]@2 3 0@2 7")  // Matches slice speed to step duration
```

### Breaks (Clean Breaks Pack)

```javascript
samples('github:yaxu/clean-breaks')
s("amen/4").fit().chop(32)  // Fit into cycles and chop into 32 pieces
```

```javascript
samples('github:yaxu/clean-breaks')
s("amen/4").fit().chop(16).cut(1)
.sometimesBy(.5, ply("2"))           // 50% chance to double
.sometimesBy(.25, mul(speed("-1")))  // 25% chance to reverse
```

```javascript
samples('github:yaxu/clean-breaks')
s("amen/4").fit()
.slice(8, "<0 1 2 3 4*2 5 6 [6 7]>*2")
.cut(1).rarely(ply("2"))
```

```javascript
samples('github:yaxu/clean-breaks')
s("amen")
.splice(8, "<0 1 2 3 4*2 5 6 [6 7]>*2")
.cut(1).rarely(ply("2"))
```

## Complete Arrangements (Drop-In Sections)

```javascript
setcpm(128/4)

// Kick and snare
$: s("bd*4, [~ sd]*2")
   .bank("RolandTR909")

// Hi-hats
$: s("hh*8").bank("RolandTR909")
   .gain("[.3 1]*4")
   .lpf(sine.range(2000,8000).slow(8))

// Bass
$: n("0 ~ 3 ~ 0 ~ 5 ~")
   .scale("E:minor")
   .s("sawtooth").oct(2)
   .lpf(800).lpq(5).shape(.3)

// Lead
$: n("7 5 7 9 7 5 3 0")
   .scale("E:minor").slow(2)
   .s("square").oct(5)
   .delay(.25).room(.5)
```

```javascript
setcpm(60/8)

// Pad layers
$: chord("<Am F C G>*0.5")
   .voicing().layer(
     x=>x.s("sine").attack(4).release(4),
     x=>x.add(7).s("triangle").attack(6).release(6).gain(.7),
     x=>x.add(12).s("sine").attack(8).release(8).gain(.5)
   ).room(.8).roomsize(10)

// Sparse melody
$: n("0 ~ ~ 4 ~ ~ 2 ~")
   .scale("A:minor").slow(4)
   .s("sine").oct(6)
   .attack(2).release(4)
   .delay(.5).room(.9)

// Texture sounds
$: s("~ ~ space ~ ~ wind ~")
   .slow(8).gain(.3)
   .lpf(rand.range(100,1000))
   .pan(rand)
```

## Generative Techniques (Careful, But Powerful)

```javascript
note("c d e f g a b c4")
.sometimesBy(.3, add(7))        // 30% chance to add 7 semitones
.rarely(rev)                    // Rarely reverse
.often(fast(2))                 // Often double speed
.sometimes(ply(2))              // Sometimes repeat notes
```

```javascript
let melody = cat(
  "c e g c4",
  "d f a d4",
  "e g b e4",
  "f a c4 f4"
).struct(pick("<x ~, x x ~, x ~ x>"))

melody.scale("C:major").s("piano")
```

```javascript
let pattern = ref()

pattern.set(
  n("0 2 4 6")
  .scale("C:major")
  .superimpose(x=>x.late(.25).add(3))  // Echo with harmony
  .sometimes(x=>x.rev())               // Sometimes reverse
  .fast(sine.range(.5,2).slow(16))     // Slow tempo variation
)
```

## Integration: MIDI / Motion / Hydra

```javascript
// Map controller to parameters
let cc = await midiin('Your Controller')
note("c e g b")
.lpf(cc(1).range(200,4000))     // CC 1 controls filter
.room(cc(2).range(0,.8))        // CC 2 controls reverb
.s("sawtooth")
```

```javascript
enableMotion()

note("0 2 4 6").scale("C:major")
.lpf(gravityY.range(200,2000))    // Tilt device for filter
.delay(accX.range(0,.5))          // Movement for delay
.s("triangle")
```

```javascript
await initHydra()

// Audio-reactive visuals
$: note("c e g b").s("sine").gain(.5)

osc().scale(()=>a.fft[0]*4+1)      // Scale visuals to audio
.color(()=>a.fft[1], 0.5, 1)      // Color from frequency bins
.out()
```
