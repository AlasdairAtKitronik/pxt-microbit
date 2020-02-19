# Measuring Stride

In order to compute the distance travelled, we need to calibrate
the relation between speed and stride. In this experiment, the runner
needs to run a distance at various speed with a constant stride.
On each run, the @boardname@ will measure the running time and the number of foot strokes.


## Find a running track!

Find a location where you can run at least 20m at a constant pace. A good example would be between the free-throw areas on a basketball backet. It is important that the runner is able to enter and leave the track with a steady speed.

Measure the distance between the start and the end of the track and store this value as ``TD``.

### ~ hint

#### Feets vs Meters

It does not matter whether you use feets or meters as long as you use the same unit everywhere!

### ~

## Code the @boardname@ s

The experiment will be operated with 2 @boardname@ that communicate via radio

* the first @boardname@ will be sleeved in the runner sock, measure the foot strikes. It runs continuously and the runner does not have to do anything, aside from focusing on his running.
* the second @boardname@ is operated on the sideline and, just like a stopwatch, measures the start and stop times of the run.

### Runner @boardname@

```blocks
input.onGesture(Gesture.ThreeG, function () {
    led.toggle(0, 0)
    radio.sendNumber(0)
})
radio.setGroup(1)
basic.showString("RUNNER")
```

### Operator @boardname@

The operator needs to press button ``A`` when the runner enters the track
and ``B`` when the runner leaves the track. Then the @boardname@ displays the measurement
two times.

```blocks
input.onButtonPressed(Button.B, function () {
    recording = false
    time = (input.runningTime() - start) / 1000
    for (let index = 0; index < 2; index++) {
        basic.showString("TIME")
        basic.showNumber(time)
        basic.showString("STEPS")
        basic.showNumber(steps)
    }
})
radio.onReceivedNumber(function (receivedNumber) {
    if (recording) {
        steps += 1
        led.toggle(0, 0)
    }
})
input.onButtonPressed(Button.A, function () {
    start = input.runningTime()
    steps = 0
    recording = true
    basic.showIcon(IconNames.Butterfly)
})
let steps = 0
let start = 0
let time = 0
let recording = false
basic.showString("STOPWATCH")
radio.setGroup(1)
```

### Recording!

Bring both @boardname@ connected to battery packs. Sleeve the runner @boardname@ into the sock of the runner and have the runner get ready to do a few runs.
The operator should be positioned so that he/she has a clear view of the start and end of the track. The operator should also have a note book or computer to write down the data of each run
* when the runner crosses the start of the track, the operator presses on button ``A``, 
* and when the runner leaves the track, he presses ``B``.
* after pressing ``B``, the @boardname@ will display the time and number of steps twice. Take not of those values in a table.

```package
radio
```