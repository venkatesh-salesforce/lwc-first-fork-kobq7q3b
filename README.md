# LWC Basics

# Why Lightning Web Components? 🤷‍♀️🤷‍♀️

Modern browsers are based on web standards, and evolving standards are constantly improving what browsers can present to a user. 
We want you to be able to take advantage of these innovations.

Lightning Web Components uses core Web Components standards and provides only what’s necessary to perform well in browsers supported by Salesforce. 
Because it’s built on code that runs natively in browsers, Lightning Web Components is lightweight and delivers exceptional performance. 
Most of the code you write is standard JavaScript and HTML.

You’ll find it easier to: 

✔ Find solutions in common places on the web.

✔ Find developers with necessary skills and experience.

✔ Use other developers’ experiences (even on other platforms).

✔ Develop faster.

✔ Utilize full encapsulation so components are more versatile, And it’s not like web components are new. 

✔ In fact, browsers have been creating these for years. Examples include <select>, <video>, <input>, and any tag that serves as more than a container. 
    These elements are actually the equivalent of web components. Our goal is to bring that level of integration to Salesforce development.

✔ In fact, Aura components can contain Lightning web components (though not vice-versa). But a pure Lightning web components implementation provides full encapsulation and evolving adherence to common standards.

 #   👏👏 Play Time

Let's go look at our files. Use an integrated development environment (IDE), to try out your components, tinker with them, and see instant results. 
If you don't already have an IDE you love, follow along as we use the third-party IDE, WebComponents.dev which includes base components and styles to help you create LWC prototypes.

✔ Navigate to webcomponents.dev and select LWC. (Or go directly to webcomponents.dev/create/lwc.) When you first get there, you see an example you can explore. 
    It includes styles from the Lightning Design System CSS framework.
    
✔ Use your GitHub account to log in to WebComponents.dev.

✔ Copy the above HTML, JavaScript, and CSS examples into the corresponding app.x files in the example. 

✔ Replace everything in the app files and save each file.

The Stories tab shows the output.  If you reopen the initial template page, you'll see the default example again.

# The LWC Module

Lightning Web Components uses modules (built-in modules were introduced in ECMAScript 6) to bundle core functionality and make it accessible to the JavaScript in your component file. The core module for Lightning web components is lwc.

Begin the module with the import statement and specify the functionality of the module that your component uses.

The import statement indicates the JavaScript uses the LightningElement functionality from the lwc module.

// import module elements
import { LightningElement} from 'lwc';
// declare class to expose the component
export default class App extends LightningElement {
    ready = false;
    // use lifecycle hook
    connectedCallback() {
        setTimeout(() => {
            this.ready = true;
        }, 3000);
    }
}

LightningElement is the base class for Lightning web components, which allows us to use connectedCallback().
The connectedCallback() method is one of our lifecycle hooks. You’ll learn more about lifecycle hooks in the next section. For now, know that the method is triggered when a component is inserted in the document object model (DOM). In this case, it starts the timer.
Lifecycle Hooks
Lightning Web Components provides methods that allow you to “hook” your code up to critical events in a component’s lifecycle. These events include when a component is:

✔ Created

✔ Added to the DOM

✔ Rendered in the browser

✔ Encountering errors

✔ Removed from the DOM

Respond to any of these lifecycle events using callback methods. For example, the connectedCallback() is invoked when a component is inserted into the DOM. The disconnectedCallback() is invoked when a component is removed from the DOM.

In the JavaScript file we used to test our conditional rendering, we used the connectedCallback() method to automatically execute code when the component is inserted into the DOM. The code waits 3 seconds, then sets ready to true.

import { LightningElement } from 'lwc';
export default class App extends LightningElement {
    ready = false;
    connectedCallback() {
        setTimeout(() => {
            this.ready = true;
        }, 3000);
    }
}


##  Note:
When you use this example in an editor like VS Code, you might see a lint warning "Restricted async operation...." for the setTimeout() function. This warning indicates you’re using an async operation that's often misused; it delays behavior based on time instead of waiting for an event. In this case, setTimeout() is suitable to demonstrate an arbitrary time delay, and the warning should not prevent you from using it.

Also, notice that we used the this keyword. Keyword usage should be familiar if you’ve written JavaScript, and behaves just like it does in other environments. The this keyword in JavaScript refers to the top level of the current context. Here, the context is this class. The connectedCallback() method assigns the value for the top level ready variable. It’s a great example of how Lightning Web Components lets you bring JavaScript features into your development. You can find a link to good information about this in the Resources section.

We’re moving fast and you’ve been able to try out some things. In the next unit, we take a step back and talk more about the environment where the components live.

# Decorators

Decorators are often used in JavaScript to modify the behavior of a property or function.

To use a decorator, import it from the lwc module and place it before the property or function.

import { LightningElement, api } from 'lwc';
export default class MyComponent extends LightningElement{
    @api message;
}
You can import multiple decorators, but a single property or function can have only one decorator. For example, a property can’t have @api and @wire decorators.

##  Examples of Lightning Web Components decorators include:

✔ @api: Marks a field as public. Public properties define the API for a component. 
        An owner component that uses the component in its HTML markup can access the component’s public properties. 
        All public properties are reactive, which means that the framework observes the property for changes. 
        When the property’s value changes, the framework reacts and rerenders the component.
        Tip Tip Field and property are almost interchangeable terms. 
        A component author declares fields in a JavaScript class. An instance of the class has properties. 
        To component consumers, fields are properties. 
        In a Lightning web component, only fields that a component author decorates with @api are publicly available to consumers as object properties.
        
✔ @track: Tells the framework to observe changes to the properties of an object or to the elements of an array.
          If a change occurs, the framework rerenders the component. All fields are reactive. 
          If the value of a field changes and the field is used in a template—or in the getter of a property used in a template—the framework rerenders the component. 
          You don’t need to decorate the field with @track. Use @track only if a field contains an object or an array and if you want the framework to observe changes to the properties of the object or to the elements of the array. If you want to change the value of the whole property, you don’t need to use @track.
          Note Prior to Spring ’20, you had to use @track to mark fields (also known as private properties) as reactive.
          You’re no longer required to do that. Use @track only to tell the framework to observe changes to the properties of an object or to the elements of an array. 
          Some legacy examples may still use @track where it isn’t needed, but that’s OK because using the decorator doesn’t change the functionality or break the code.
          For more information, see this release note.
          
✔ @wire: Gives you an easy way to get and bind data from a Salesforce org.
