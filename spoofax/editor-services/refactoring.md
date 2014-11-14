# Refactorings

Refactorings are used for transformations that modify the original source code, 
  while preserving the layout and syntactic sugar. 

## Implementation

Refactorings are implemented as transformations in Stratego. 
The implementing rewrite rule needs to have the following structure:

    my-refactor-transformation: 
    (user-input, selected, position, ast, path, project-path) -> 
    (changes, fatal-errors, errors, warnings)

## Editor Specification

Refactorings are typically specified in `editor/YourLang-Refactorings.esv` as follows:

    refactorings
      
      refactoring Property*: "Extract Entity" =  extract-entity (source)
        shortcut: Shift+Alt+M
        input
          identifier: "entity name" = ""
          identifier: "property name" = ""
  
In this example, 
  `Property*` indicates on which constructs the refactoring is specified (`<Symbol>` or `<Sort>.<Constructor>`),
  the caption `"Extract Entity"` is used in the refactor menu, and
  the `extract-entity` transformation is used to perform the refactoring.
  Additional elements specify a shortcut and an input dialog.
  
### Annotations

Refactoring specifications can include any combination of the following annotations:
* `(cursor)`: The refactoring should always transform the tree node at the cursor.
* `(meta)`: Indicates the refactoring should only be available to language engineers
  (i.e., not when the language is deployed to language users).
* `(source)`: Always apply this refactoring to the source AST, not to the AST after it has been analyzed by the observer.

### Input Dialogue

Refactoring `input` definitions can specify any combination of input field types,
adhering to the general schema 

    <input-type> : <label-text> = <default-value> 


In the example, the user input dialogue contains two `identifier` input fields.
The following input types are supported:

* `text` for text input
* `identifier` for text input with syntactic validity check, i.e. the value must be a valid identifier
* `boolean` for checkbox input, (default) values are `true` or `false`

### Shortcuts and Keybindings

Shortcuts can be added to refactorings directly, e.g.

    shortcut: Shift+Alt+M

Furthermore, you can define named keybindings in a `keybindings` section.
Each binding follows the general schema

    <Key> + <Key> + <Key> = "<action-definition-id>"
  
We predefined a number of common refactoring shortcuts, see `editor/YourLang-Refactorings.generated.esv` for details.
You can use named keybindings as shortcuts:

    shortcut: "org.eclipse.jdt.ui.edit.text.java.extract.method"
  
### Text Reconstruction

Some additional strategies can be specified in the `refactorings` section to guide the text reconstruction algorithm.

* `pretty-print`: Newly inserted AST nodes are formatted according to a given pretty-print strategy.
* `resugar`: Resugaring can be enforced with a strategy that undoes the desugaring. 
  Resugaring is needed for desugarings that are not local-to-local, or 
  desugarings that duplicate terms.
* `override reconstruction`: Text reconstruction can be overruled with a custom strategy.
  The resulting text will be indented according to its location.
