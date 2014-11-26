# Menus
Spoofax allows you to define menus containing actions. These actions are typically transformations on the AST and the result is typically written to a file. However, you may defer from the typical use of actions and, for example, have an action that invokes some Java code by means of a [Java Strategy](http://strategoxt.org/Spoofax/Tour#Adding_Java_components).
In Eclipse, the defined menus will appear in the menus toolbar and are dynamically added/removed upon building the project. Menus are defined in `editor/YourLang-Menus.esv`.

## Available menu constructs

The example below shows the various constructs that are available: menus, submenus, actions, separators and icons. 

    menus
      
      menu: "Actions"                    (openeditor)
        
        action: "Format"               = editor-format (realtime) (source)
        
        separator
        
        action: "Show abstract syntax" = debug-show-aterm (realtime) (meta) (source)
        action: "Show analyzed syntax" = debug-show-analyzed (meta)
        
        submenu: "Generate"
          action: "Generate Java"      = generate-java
          action: "Generate C++"       = generate-cpp
        end
      
      menu: Icon("icons/foo.bar")        (meta)
      
The rules are as follows:

 - A menu either has a label (e.g. `"Actions"`) or an icon (e.g. `Icon("icons/foo.bar")`)
 - An action has a label (e.g. `"Format"`) and a strategy (e.g. `editor-format`).
 - Menus contain submenus, actions and separators. 
 - Submenus contain other submenus, actions or separators.

## Options on actions

The available options on actions are as follows:

 - `(meta)` makes the actions only available to meta-programmers (i.e. they are not available when the plugin is deployed to end-users).
 - `(openeditor)` indicates an editor should be opened with the result.
 - `(realtime)` indicates that the resulting editor should be updated in real-time as the source file is edited.
 - `(source)` applies the action to the source AST and not the analyzed one.
 - `(cursor)` transforms the tree node at the cursor (i.e. even if the selection is empty).

Options may also be defined on menus or submenus, in which case they propagate to their contained actions and submenus.

## Builder definition

For each action, a builder should be defined in Stratego as shown below. The builder takes a five-tuple (e.g. `(selected, position, ast, path, project-path)`) and should produce a two-tuple `(path, result)` if the result should be persisted or `None()` otherwise.

    generate-java:
      (selected, position, ast, path, project-path) -> (path', result)
      with
        path'  := <guarantee-extension(|"java")> path;
        result := <to-java> selected
        

