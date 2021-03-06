# structure assertions

Many projects nowadays work with a centralized asset library that sports css and/or js components.
In many cases this library/framework is created from ux/marketing/whatever-agencies or much better in your own responsibility,
but in either way you are facing the same kind of problems.

An HTML interface contract is in place that binds the components maintained in the centralized repository
to the implementations created in apps that include this repository. Once the framework is released in a new version
this contract may be violated. Evolving designs may involve demands to the html structure they are applied on,
so that html and css usually have to be adjusted side to side.

In environments where frontend development is done beforehand or in a central repository,
so where side by side development is not possible we need a way to get in touch with those components
that become out of sync to the actual implementation.

## usage

### node

t.b.a

### browser

See that you have expect.js expect-dom.js and the structure-assertions.js libs available:

```html
<script src="./node_modules/expect-dom/vendor/expect.js"></script>
<script src="./node_modules/expect-dom/expect-dom.js"></script>
<script src="./structure-assertions.js"></script>
```

### demo

have a look at the [demo](https://mechanoid.github.io/structure-assertions/demo.html) page,
with assertion errors highlighted with tooltipster - tooltips.

### examples

#### in general

You may add some component assertions to your page (usually only in dev or test environments),
that will cry out for components that are out of sync to the frontend lib.

```js
assert('.awesome-component').toHave( function(expect) {
  expect.to.have.attr("data-awesomeness");
  expect.to.containChild('.awesome-component-footer');
});
```

#### optionals

In detail that means that we want to describe rules about our JS and CSS components,
that are must haves and can be asserted. While those assertion make a good deal about documentation,
they are not documenting all possibilities for your component.

While assertions are about breaking changes and must haves, where we want to see directly when we are breaking the contract,
assertions are not telling us if we have certain optional possibilities for a component. Therefore structure-assertions
may have additional optionals defined that are available on the asserted object and will be printed to the debug console for default.

```js
assert('.awesome-component').toHave( function(expect) {
    expect.optional.classes('data-awesome-default', 'data-awesome-danger', 'data-awesome-warn');
    expect.optional.attributes('data-awesome');
});
```


#### for something like twitters Bootstrap

With this in mind imagine a library of structure-assertions for [Bootstrap](http://getbootstrap.com/),
that may look like that.

**list-group**

```html
<ul class="list-group">
  <li class="list-group-item">Cras justo odio</li>
  <li class="list-group-item">Dapibus ac facilisis in</li>
  <li class="list-group-item">Morbi leo risus</li>
  <li class="list-group-item">Porta ac consectetur ac</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>
```

```js
assert('.list-group').toHave( function(expect) {
  expect.to.be.tag('ul,ol');
  expect.to.containChild('.list-group-item');
});

assert('.list-group-item').toHave( function(expect) {
  expect.to.be.tag('li');
});
```

**panel**

```html
<div class="panel panel-danger">
 <div class="panel-heading">Panel heading without title</div>
 <div class="panel-body">
   Panel content
 </div>
</div>
```

```js
assert('.panel').toHave( function(expect) {
  expect.to.containChild('.panel-heading,.panel-body');
  expect.optional.classes('panel-primary', 'panel-success', 'panel-info', 'panel-warning', 'panel-danger');
  expect.optional.children('.panel-footer');
});

assert('.panel-danger').toHave( function(expect) {
  expect.to.be.deprecated();
});

assert('.panel-heading').toHave( function(expect) {
  expect.to.be.tag('div');
}

assert('.panel-body').toHave( function(expect) {
  expect.to.be.tag('div');
}
```

## Credits

The MIT License (MIT)

Copyright (c) 2015 Falk Hoppe <falkhoppe81@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

### 3rd-party

Heavily borrows from expect.js by Guillermo Rauch - MIT,
expect-dom.js by Kevin Dente - MIT
and should.js by TJ Holowaychuck - MIT
