# Cypress.io Study Notes

Cypress is a JavaScript and Chrome only tools that uses a real browser to run e2e tests. Cypress also works upon Mocha.js.

## Installing Cypress

To run tests in cypress you need to install the modern javascript tools.

### Requisites
- Node.js

In the folder that you will use cypress run the have to run the following commands:

```
npm init -y
npm install cypress
```

To run cypress run `npx cypress open`, this command will open a cypress console where run all of your tests.

## Writing first test in Cypress

```javascript
// This code is a simple test in cypress.
// The test files in cypress should have the extension .spec.js.

// the "it" method specifies a test unit and receives the test
// name and a function with the text.
it('should navigate to the TodoMVC App', () => {

    // The "cy" object represents a cypress and contains all
    // of the methods that we can use to interact with the
    // browser (chrome) and make the test. The "visit" open
    // in the browser the page that you specify as parameter.
    cy.visit('http://todomvc-app-for-testing.surge.sh/');
});
```

## Interacting With elements

```javascript
it('should navigate to the TodoMVC App', () => {
    cy.visit('http://todomvc-app-for-testing.surge.sh/');

    // The "get" method use a CSS selector to find a 
    // element in the DOM browser
    cy.get('.new-todo')
    // The "type" write that you specify as a first argument.
    // The '{enter} sends a `Enter` character to the browser "
        .type('Clean room{enter}');
});
```

### Set a timeout

Cypress by default have a 4000 milliseconds timeout to make a test. The next sample shows how to work in tests that takes more than 4000 milliseconds.

```javascript
it('should navigate to the TodoMVC App', () => {
    // This is a timeout url
    cy.visit('http://todomvc-app-for-testing.surge.sh/?delay-new-todo=5000');

    // Set a timeout to get the element.
    cy.get('.new-todo', { timeout: 6000 })
        .type('Clean room{enter}');
});
```

### Select by tests

```javascript
it('should navigate to the TodoMVC App', () => {
    cy.visit('http://todomvc-app-for-testing.surge.sh/');

    // The "contains" methods get a element by the text that it displays.
    cy.contains('Clear completed').click();
});
```

## Assertions

```javascript
// This example cover some assertions of Cypress
it('should navigate to the TodoMVC App', () => {
    cy.visit('http://todomvc-app-for-testing.surge.sh/');

    cy.get('.new-todo')
        .type('Clean room{enter}');
    
    // the "should" method is to set assertions obtained 
    // by the "get" method.
    cy.get('label').should('have.text', 'Clean room');
    cy.get('.toggle').should('not.be.checked');

    cy.get('.toggle').click();
    cy.get('label').should('have.css', 'text-decoration-line', 'line-through');

    cy.contains('Clear completed').click();

    cy.get('.todo-list')
        .should('not.have.descendants', 'li');
});
```

### Grouping tests with Mocha

```javascript
// The "describe" method is use to group a tests suite.
describe('todo actions', () => {

    // The "it" method is to set a test inside a test suite.
    it('should add a new todo to the list', () => {
        cy.visit('http://todomvc-app-for-testing.surge.sh/');
    
        cy.get('.new-todo')
            .type('Clean room{enter}');
        
        cy.get('label').should('have.text', 'Clean room');
        cy.get('.toggle').should('not.be.checked');
    
    });
    
    it("should mark a todo as completed", () => {
        cy.get('.toggle').click();
        cy.get('label').should('have.css', 'text-decoration-line', 'line-through');
    });
    
    it('should clear completed todos', () => {
        cy.contains('Clear completed').click();
    
        cy.get('.todo-list')
            .should('not.have.descendants', 'li');
    });
});
```

We can use run commands before each tests using the `beforeEach` method.

```javascript

describe('todo actions', () => {

    // Below we specify all the commands to be run before each tests.
    beforeEach(() => {
        cy.visit('http://todomvc-app-for-testing.surge.sh/');
        cy.get('.new-todo')
            .type('Clean room{enter}');
    });

    it('should add a new todo to the list', () => {
        cy.get('label').should('have.text', 'Clean room');
        cy.get('.toggle').should('not.be.checked');

    });

    it("should mark a todo as completed", () => {
        cy.get('.toggle').click();
        cy.get('label').should('have.css', 'text-decoration-line', 'line-through');
    });

    it('should clear completed todos', () => {
        cy.get('.toggle').click();
        cy.contains('Clear completed').click();

        cy.get('.todo-list')
            .should('not.have.descendants', 'li');
    });
});
```

## Cypress CLI

To run cypress no-interactive we need to run `npx cypress run`.

## Configure Cypress

We can create a `cypress.json` in the project root to specify the cypress config. In this JSON file you can add the next values.

* `watchForFileChanges`: This property turns the cypress to watch the files setting a boolean value.