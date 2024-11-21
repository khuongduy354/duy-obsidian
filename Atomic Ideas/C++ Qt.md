# how qt compile 
.proj -> (cmake) makefile -> run make 
- cmake used to make simple Makefile, and extract meta-objects data 
# Basic usage
 ```cpp 
//tr 
// multiple language support for a string   
tr("Hello World") 

// hierarchy 
 QPushButton button1 ("test");
 QPushButton button2 ("other", &button1);  

 button1 is parent of button2 

//CONNECT SLOTS 
//everything must be a pointer, or a reference to main object
connect(buttonBox,&QDialogButtonBox::accepted,this,&DetailedDialog::verify);  

//or 
QLabel *label = new QLabel;
QScrollBar *scrollBar = new QScrollBar;
QObject::connect(scrollBar, SIGNAL(valueChanged(int)),
                 label,  SLOT(setNum(int)));
```
# APIS 
```cpp
    QLabel *nameLabel;
    QLabel *addressLabel;
    QLineEdit *nameEdit;
    QTextEdit *addressEdit;

    QTableWidget *itemsTable; 
    QStringList items; //array of string 

    QCheckBox *offersCheckBox;
    QDialogButtonBox *buttonBox; //can make multiple buttons at once   
    
    QGridLayout //use coordinates to place items
	QTableWidget //like above,but more numeric (data chart)
```








# C++ Qt
--- 
# Refererences 




2023 01 03 22:14
#cheatsheet   [[C++ Essence]] 