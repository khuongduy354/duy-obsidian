2022 07 14 16:35
Status: #concept 
Tags: [[design pattern]]
# Factory

# Factory

> Create object indirectly (without new keyword), but from a function
> 

Reason ? To allow the subclass change the parent class

# Problem

Imagine that you’re creating a logistics management application. The first version of your app can only handle transportation by trucks, so the bulk of your code lives inside the `Truck` class.

After a while, your app becomes pretty popular. Each day you receive dozens of requests from sea transportation companies to incorporate sea logistics into the app.

- **Guidelines**
    
    A **Creator** class makes **ConcreteCreator** class 
    
    **ConcreteCreator** make **products** use the **new** keyword through a **factory method**
    
    -Don’t make objects from **Creator** class directly (with **new)** 
    
    → Can change **products** make by a class through its subclass  (the **ConcreteCreator)**
    
- **Short Visualization**
    
    Goal: A class return an object with type interface
    
    But don’t make it directly
    
    Use another class to inherit above, and then return object (1) 
    
    we can alter behavior in (1) 
    

# Implementation

## How

1. **Products** follow same **interface**
2. **Creator** has empty method, with return type **product interface**
3. All **products** made through a **factory** method (through a **subclass**)

- notes
    1. The **Product** declares the interface, which is common to all objects that can be produced by the creator and its subclasses.
    2. **Concrete Products** are different implementations of the product interface.
    3. The **Creator** class declares the factory method that returns new product objects. It’s important that the return type of this method matches the product interface.

- others diagram
- Code
    - pseudo
        
        ```java
        // The creator class declares the factory method that must
        // return an object of a product class. The creator's subclasses
        // usually provide the implementation of this method.
        class Dialog is
            // The creator may also provide some default implementation
            // of the factory method.
            abstract method createButton():Button
        
            // Note that, despite its name, the creator's primary
            // responsibility isn't creating products. It usually
            // contains some core business logic that relies on product
            // objects returned by the factory method. Subclasses can
            // indirectly change that business logic by overriding the
            // factory method and returning a different type of product
            // from it.
            method render() is
                // Call the factory method to create a product object.
                Button okButton = createButton()
                // Now use the product.
                okButton.onClick(closeDialog)
                okButton.render()
        
        // Concrete creators override the factory method to change the
        // resulting product's type.
        //In python this parts can be rewritten into 1 single function called Factory
        class WindowsDialog extends Dialog is
            method createButton():Button is
                return new WindowsButton()
        
        class WebDialog extends Dialog is
            method createButton():Button is
                return new HTMLButton()
        
        // The product interface declares the operations that all
        // concrete products must implement.
        interface Button is
            method render()
            method onClick(f)
        
        // Concrete products provide various implementations of the
        // product interface.
        class WindowsButton implements Button is
            method render(a, b) is
                // Render a button in Windows style.
            method onClick(f) is
                // Bind a native OS click event.
        
        class HTMLButton implements Button is
            method render(a, b) is
                // Return an HTML representation of a button.
            method onClick(f) is
                // Bind a web browser click event.
        
        class Application is
            field dialog: Dialog
        
            // The application picks a creator's type depending on the
            // current configuration or environment settings.
            method initialize() is
                config = readApplicationConfigFile()
        
                if (config.OS == "Windows") then
                    dialog = new WindowsDialog()
                else if (config.OS == "Web") then
                    dialog = new WebDialog()
                else
                    throw new Exception("Error! Unknown operating system.")
        
            // The client code works with an instance of a concrete
            // creator, albeit through its base interface. As long as
            // the client keeps working with the creator via the base
            // interface, you can pass it any creator's subclass.
            method main() is
                this.initialize()
                dialog.render()
        ```
        
    - python
        
        ```python
        # Python Code for factory method
        # it comes under the creational
        # Design Pattern
         
        class FrenchLocalizer:
         
            """ it simply returns the french version """
         
            def __init__(self):
         
                self.translations = {"car": "voiture", "bike": "bicyclette",
                                     "cycle":"cyclette"}
         
            def localize(self, message):
         
                """change the message using translations"""
                return self.translations.get(msg, msg)
         
        class SpanishLocalizer:
            """it simply returns the spanish version"""
         
            def __init__(self):
                self.translations = {"car": "coche", "bike": "bicicleta",
                                     "cycle":"ciclo"}
         
            def localize(self, msg):
         
                """change the message using translations"""
                return self.translations.get(msg, msg)
         
        class EnglishLocalizer:
            """Simply return the same message"""
         
            def localize(self, msg):
                return msg
         
        def Factory(language ="English"):
         
            """Factory Method"""
            localizers = {
                "French": FrenchLocalizer,
                "English": EnglishLocalizer,
                "Spanish": SpanishLocalizer,
            }
         
            return localizers[language]()
         
        if __name__ == "__main__":
         
            f = Factory("French")
            e = Factory("English")
            s = Factory("Spanish")
         
            message = ["car", "bike", "cycle"]
         
            for msg in message:
                print(f.localize(msg))
                print(e.localize(msg))
                print(s.localize(msg))
        ```
        

# Applicability

1.  Don’t know the exact **types** and **dependencies.**
2. **Extending** internal components ( extendable library,...) 
3. **Reusing**

# Cons

1. A sub-class for each type → Huge amount of small files








--- 
# Refererences 
