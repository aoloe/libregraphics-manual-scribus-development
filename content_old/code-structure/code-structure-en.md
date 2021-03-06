# The Scribus Code Structure

This draft has been build upon elvis mail to the scribus-dev mailing list

[scribus-dev] Some notes/questions about Scribus internals and my project

## Central Classes

These are notes on (a few of) the central classes of Scribus. I've
excluded the styles system as it has kind of OK documentation in
styles/overview.txt.

## ScribusDoc

Self-explanatory. Represents a document in Scribus. Some
responsibilities seems to be:

- get a list of objects in the document.
- get the current selection.
- manage styles and colors.
- manage grid and guides.

## ScribusView

Central zoomable scrollarea showing a document. Its primary
responsibility seems to be to provide a UI for scrolling and zooming
(naturally).

## PageItem and its subclasses

PageItem is the base class for all items. The base class seems to be a
bit intertwined with its subclasses, and can be queried for and
converted to its subclass type using is*() and as*() methods,
respectively (e.g. isArc(), asArc()). Page items have many
responsibilities, but most important for me seems to be that they draw
themselves to the canvas in their DrawObj_Item() method.

## UndoManager

Central singleton class of the undo/redo system. Manages a map of undo
stacks, one for each document. I'm not 100% sure how the undo system
works as it does not seem to use the traditional command pattern for
undo/redo. But I understand that the undo stacks are stacks of
UndoState:s, and that multiple UndoStates can be packed into a
transaction which can be commited or rolled back.

I wonder why it was made into a singleton with several stacks (one per doc) instead of
having one undo manager per document?
Answer: There may be different documents, but we only have one UndoAction linked to
the UI.
So at one point we need to decide which document to use anyway.


Also, even though it is a singleton, pointers to the undo manager still seems to be kept in
classes such as ScribusView and ScribusMainWindow. But I guess this is
just for convenience?

Undo is based ona quite simple in idea - in UndoState are saved changed parameteres of 
item before and after changing. For example for undow moving PageItem in undo 
state is saved its start position and end position. On undo start position of 
PageItem will be restored. On redo end position will be restored again.

It is actually a variant of the command pattern. SimpleState is
more or less a "one-size-fits-all" kind of Command pattern implementation.
Scribus should use less SimpleState and more specific subclasses
of UndoState. IIRC it's not necessary to implement the restore method in the
UndoObject in this case.

In implementation undo can be more complicated because in undostate only 
simple variables types can be saved - numbers and strings.
So for text undo, where in undostate must be saved text with all styles 
information, I prepare function which return all that information in string by 
serializing it ("saxing", like in SLA file, when all information is in human readable strings 
in tagged fashion, in a somehow XML-ish manner).





## General Notes

Many of the classes such as ScribusDoc, ScribusView and PageItem have
a huge number of public member variables, many of them undocumented,
which users (and subclasses) of the classes rely on, and it's a bit
hard to tell what the responsibilities of each class really is. But I
think I have a somewhat OK view of it now.

Isn't that ugly? :-) It's on the roadmap for refactoring.


## CanvasMode

Different modes of operations on the canvas seems to be implemented in
classes inheriting from CanvasMode. As an example, there is a canvas
mode for editing a polygon, editing a spiral et.c.

At a even higher level there seems to be the concept of "application
modes" (e.g. `ScribusMainWindow::setAppMode(int)`), which I'm not quite
sure what they are. Are they the same as canvas modes? In any case,
setting the "app mode" using ScribusMainWindow::setAppMode(int) seems
to be what results in a canvas mode change.

In the beginning Scribus just had application modes and some huge switches
and if-then-else constructs to do the right thing at the right time. I
refactored all that code into CanvasModes (correspond to application modes) and
CanvasGestures (correspond to submodes, ie. Scribus returns to the CanvasMode it was in
before the CanvasGesture was started. Gestures could be stacked).

This structure made it possible to concentrate on a single user interaction
when implementing mouse event code.

## Notes on Painting of Items

Painting seems to be done in two places. The on-canvas painting of
items seems to be done by items themselves in their DrawObj_Item()
(and DrawObj_Decoration()) method. Painting of items for print output
seems to be done in the respective drawItem_*() methods in
ScPageOutput.

IIRC ScPageOutput is only used for printing in Scribus. There's additional
code in pdflib_core.cpp and pslib.cpp (Yes, 4 versions of the same code. That's
another refactoring on the roadmap!) 
