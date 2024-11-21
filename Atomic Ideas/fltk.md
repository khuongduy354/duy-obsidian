# worst thing 
callback has to be static or global, cant be a class member
```c++ 
#include <FL/Fl.H>
#include <FL/Fl_Box.H>
#include <FL/Fl_Window.H>

int main(int argc, char **argv) {
  Fl_Window *window = new Fl_Window(300, 180);  

  //window's children
  Fl_Box *box = new Fl_Box(20, 40, 260, 100, "Hello, World!");
 
  box->box(FL_UP_BOX);
  box->labelsize(36);
  box->labelfont(FL_BOLD + FL_ITALIC);
  box->labeltype(FL_SHADOW_LABEL);

  //not allow as window's children 
  window->end();   

  window->show(argc, argv);
  return Fl::run();
}
``` 













# fltk
--- 
# Refererences 




2023 02 26 11:06
#ideas  [[Atomic Ideas/C++]] [[C++ Essence]] [[GUI]]