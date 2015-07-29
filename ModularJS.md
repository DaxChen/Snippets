# Modular Way of Writing JS
https://youtu.be/m-NYyst_tiY?list=PLoYCgNOIyGACnrXwo5HMCfOH9VT05znGv

Get rid of those speghetti codes!


## Some ground rules
- self-contained module
  - everything to do with my module is in my module
  - no global variables
  - if a module manages more than one thing it should split up
- separating of concerns
- DRY code: Don't Repeat Yourself
- efficient DOM usage
  - very few $(selections)
- no memory leaks
  - all events can be unbound


## The Pattern!

```javascript
(function() {

   var myModule = {
      variableOne: [],
      varTwo: 'hahaha',
      init: function() {
         this.cacheDom();
         this.bindEvents();
         this.render();
      },
      cacheDom: function() {
         this.$el = $('#peopleModule');
         this.$button = this.$el.find('button');
         this.$input = this.$el.find('input');
         this.$ul= this.$el.find('ul');
         this.template = this.$el.find('#people-template').html();
      },
      bindEvents: function() {
         this.$button.on('click', this.addPerson.bind(this));
         this.$ul.delegate('i.del', 'click', this.deletePerson.bind(this));
      },
      render: function() {
         var data = {
            people: this.people
         };
         this.$ul.html(Mustache.render(this.template, data));
      },
      addPerson: function() {
         this.people.push(this.$input.val());
         this.render();
         this.$input.val('');
      },
      deletePerson: function(event) {
         var $remove = $(event.target).closest('li');
         var i = this.$ul.find('li').index($remove);

         this.people.splice(i, 1);
         this.render();
      }
   };

   myModule.init();

})()
```

this is good but either the whole module is accessable or hidden, how about some private and some api?

## Revealing Module Pattern 

```javascript
var myModule = (function() {
   var privateVariableOne = ['Will', 'Steve'];

   // cache Dom
   var $el = $('#peopleModule');
   var $button = $el.find('button');
   var $input = $el.find('input');
   var $ul= $el.find('ul');
   var template = $el.find('#people-template').html();
   
   // bindEvents
   $button.on('click', addPerson);
   $ul.delegate('i.del', 'click', deletePerson);

   function _render() {
      $ul.html(Mustache.render(template, {people: people}));
   }

   function addPerson(value) {
      var name = (typeof value === 'string') ? value : $input.val();
      people.push(name);
      _render();
      $input.val('');
   }

   function deletePerson(event) {
      var i;
      if (typeof event === 'number') {
         i = event;
      } else {
         var $remove = $(event.target).closest('li');
         i = $ul.find('li').index($remove);
      }

      people.splice(i, 1);
      _render();
   }

   return {
      addPerson: addPerson,
      deletePerson: deletePerson
   };

})();
```
