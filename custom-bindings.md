# Custom Bindings

<br />

<p id="first-p">
In some of your applications that use the Varbox platform, you might find the need to overwrite, customize or extend functionalities that come with the platform, in order to match your needs.
</p>

In its underlying implementation, the Varbox platform uses a lot of classes, each serving its own purpose in different parts of the system. 

Throughout these classes, the following support being overwritten by you:

<style>
    #available-filter-operators-list > p {
        column-count: 3; -moz-column-count: 3; -webkit-column-count: 3;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    #available-filter-operators-list span {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 14px;
        color: #4AAEE3;
    }
</style>
<div id="available-filter-operators-list" markdown="1">
<span>services</span>
<span>models</span>
<span>controllers</span>
<span>form requests</span>
<span>view composers</span>
<span>middleware</span>
<span>helpers</span>
<span>filters</span>
<span>sorts</span>
</div>

> A very important thing to note here is that you can overwrite any of these platform classes with your own implementations.

You can find all platform classes that support overwrites inside the `config/varbox/bindings.php` config file.
You should update the config key of the class you want to overwrite with your own class' fully qualified namespace.

> It's very encouraged that you read the doc block comments above the config key you wish to overwrite, because it provides useful insight on how to extend it.

When overwriting a platform class, you have two options: 
- extend the platform class itself and add or modify behaviors
- implement an entirely new class, but implement the contract

Either way, you're required to implement the `contract` *(if the original platform class implements one)*, because the platform, at `service provider` level, will bind a concrete implementation to that contract, so without the contract implemented, nothing will happen.
