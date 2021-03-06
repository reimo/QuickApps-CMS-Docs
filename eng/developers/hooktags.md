Hooktags
========

A `Hooktag` is a QuickApps-specific code that lets you do nifty things with very little effort.  
Hooktags can for example print current language code/name/nativeName or call especifics modules/themes functions.  
For example, the Block module has the 'block' hooktag which will print out the indicated block by id:

    [block id=1 /] 

Wherever you write (node content for example) the code above, and if hooktags are allowed, it will be replaced by the HTML block code.


Creating Hooktags
=================

Hooktags are Helpers methods, they may be located in each Module or Theme `HooktagsHelper` class.  
For example, you may have a module named `HotModule`, and its Hooktag Helper class is:

    ROOT/Modules/HotModule/View/Helper/HotModuleHooktagsHelper.php:
    class HotModuleHooktagsHelper extends AppHelper {
    }

A hooktag handler method should accept one to three arguments: 

* **$atts** (first argument): an associative array of attributes
* **$content** (second argument): the enclosed content (if the hooktag is used in its enclosing form)
* **$code** (third argument): the hooktag name (only when it matches the callback name)


Attributes
==========

The **$atts** array may include any arbitrary attributes that are specified by the user.  
Attribute names are always converted to lowercase before they are passed into the handler function.
Values are untouched. [some_hooktag  Foo="bAr"] produces $atts = array('foo' => 'bAr').

**TIP: Don't use camelCase or UPPER-CASE for your $atts attribute names**


---


Example: Creating a Hooktag
---------------------------

Lets create a hooktag for displaying HTML content-boxes.

Our hooktag:

 * Name will be `content_box`.
 * Will use the `enclosed` form ([tag] ... [/tag]), for holding the box's content.
 * Will accept a `color` parameter for specify the color of the box to render.
 * Will be handled by the module `HotModule`.


---
Basically our hooktag must convert the code below:

> [content_box color=green]Lorem ipsum dolor[/content_box]

To its HTML representation:

    <div class="box-green">
        Lorem ipsum dolor
    </div>
---

Now we must create a hooktag handler method named `content_box`, and it should receive at least two arguments ($attr, $content).

    // ROOT/Modules/HotModule/View/Helper/HotModuleHooktagsHelper.php:
    class HotModuleHooktagsHelper extends AppHelper {
        public function content_box($atts, $content=null, $code="") {
            $return = '<div class="dialog-' . $atts['color'] . '>';
            $return .= $content;
            $return .= '</div>';

            return $return;
        }
    }

**Usage:**
Now you are able to use it in any Node's contents, or wherever hooktags are allowed.


> [content_box color=green]Lorem ipsum dolor[/content_box]

Wherever you place the code above it will replaced by the following HTML code:

    <div class="dialog-green">Lorem ipsum dolor</div>