% testing physics


~~~~
(define marbleRadius 8)
(define xCenter (/ worldWidth 2))
(define nrow 7)
(define ncol 7)
(define binWidth (/ worldWidth ncol))

(define (marbleX) (+ xCenter (uniform -1 1))) ;*almost* the center

;the pegs, walls, and bins of the plinko machine
(define emptyPlinko (plinkoWorld nrow ncol))

;place a marble at the top and almost at the center
(define (setupPlinko) (addCircle emptyPlinko (marbleX) 0 marbleRadius #f))

(define (dropMarble world) (runPhysics 1000 world))
(map dropMarble (repeat 1 setupPlinko))

;(hist (map plinkoWhichBin (map (lambda (w) (runPhysics w)) (repeat 100 setupPlinko))))

;run physics and find out what bin the marble fell into
;note: run physics now returns the final world, followed by the initial world
;(so randomization is recorded)
;(define (runPlinko) (plinkoWhichBin (runPhysics 1000 world) ncol))

;(hist (repeat 100 runPlinko) "Plinko")

~~~~

Same model with animation:

~~~~
(define marbleRadius 8)
(define xCenter (/ worldWidth 2))
(define nrow 7)
(define ncol 7)
(define binWidth (/ worldWidth ncol))

(define (marbleX) (+ xCenter (uniform -1 1)))

(define world (addCircle (plinkoWorld nrow ncol) (marbleX) 0 marbleRadius #f))
(animatePhysics 1000 world)

~~~~

Random falling things:

~~~~
(define (dim) (uniform 5 20))
(define (xPos) (uniform 0 worldWidth))
(define (yPos) (uniform 0 worldHeight))

(define groundedWorld (addRect emptyWorld
                               (/ worldWidth 2)
                               worldHeight
                               worldWidth
                               10
                               #t))

(define (addRndCircle w) (addCircle w (xPos) (yPos) (dim) #f))
(define (addRndRect w) (addRect w (xPos) (yPos) (dim) (dim) #f))

(define world (addRndCircle (addRndRect (addRndCircle groundedWorld))))

(animatePhysics 1000 world)

~~~~

Towers:

~~~~
(define towerWorld (makeTowerWorld))
(animatePhysics 1000 towerWorld)

~~~~

Tower Stability:

<!--
~~~~
(define (getY obj) (second (second obj)))
;y position is 0 at the TOP of the screen
(define (highestY world) (min (map getY world)))

(define eps 10) ;things might move around a little, but within 10 pixels is close
(define (approxEqual a b) (< (abs (- a b)) 10))

(define (doesTowerFall initialW finalW) (not (approxEqual (highestY initialW)
                                                          (highestY finalW))))


(define initW (makeTowerWorld))
(define finalW (runPhysics 1000 initW))
(define initYs (map getY initW))
(define finalYs (map getY finalW))

;(define (towerRun initialW) (doesTowerFall initialW
;                                           (runPhysics 1000 initialW)))
;(define initialWorlds (repeat 10 makeTowerWorld))
;(define towerResults (map towerRun initialWorlds))

(list (min initYs)
      (min finalYs))


~~~~
-->

~~~~
(define (getY obj) (second (second obj)))
;y position is 0 at the TOP of the screen
(define (highestY world) (min (map getY world)))

(define eps 10) ;things might move around a little, but within 10 pixels is close
(define (approxEqual a b) (< (abs (- a b)) 10))

(define (doesTowerFall initialW finalW) (not (approxEqual (highestY initialW)
                                                     (highestY finalW))))

(define initialWorld (makeTowerWorld))
(define finalWorld (runPhysics 1000 initialWorld))

(doesTowerFall initialWorld finalWorld)

~~~~

Counter-Inutitively Stable Structure:

~~~~
(define almostUnstableWorld (list (list (list 'rect #t (list 350 5))
                                        (list 175 500))
                                  (list (list 'rect #f (list 24 22))
                                        (list 175 473))
                                  (list (list 'rect #f (list 15 38))
                                        (list 159.97995044874122 413))
                                  (list (list 'rect #f (list 11 35))
                                        (list 166.91912737427202 340))
                                  (list (list 'rect #f (list 11 29))
                                        (list 177.26195677111082 276))
                                  (list (list 'rect #f (list 11 17))
                                        (list 168.51354470809122 230))))
(animatePhysics 1000 almostUnstableWorld)

~~~~

Same structure, this time with noise:


