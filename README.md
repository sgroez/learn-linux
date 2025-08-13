# learn-linux
Documentation of my learning progress with low level linux concepts and the modules using them.

## Chapters
- [Kernel locking/atomic](locking_atomic.md)
- [Interrupts](interrupts.md)

## Terminology and Abbreviations
UMA | Unified Memory Architecture  
KMS | Kernel Mode Setting  
page flip | update from second buffer to avoid tearing when updating displayed buffer  
vblank | trigger to render new frame  
plane | planes the graphics engines can draw to (get combined to final output image)  
GEM | Graphics Execution Manager / simple memory management library (only used on UMA)  
TTM | Translation Table Manager / complex but universal memory management library  

## Sources
- https://docs.kernel.org

