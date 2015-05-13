class: center, middle

# Ember Improved Actions ✨

---

# Les Actions aujourd’hui

--

### Dans un template

```handlebars
<!-- template.hbs -->
<button {{action "submit"}}>Save</button>
```

--

```javascript
// controller.js
export default Ember.Controller.extend({
  actions: {
    submit: function(){
      // some submission task
    }
  }
});
```

---

# Les Actions aujourd’hui
### En utilisant un Component

```handlebars
<!-- template.hbs -->
{{my-control on-click="submit"}}
```

```javascript
// app/components/my-control/component.js
export default Ember.Component.extend({
  click: function(){
    this.sendAction('on-click');
  }
});
```

---

# Limitations : imbrication pénible

---

# Limitations : imbrication pénible

.center[
```handlebars
{{my-form}} ← {{my-control}} ← {{my-button}}
```
]

---

# Limitations : imbrication pénible

```javascript
// app/index/my-button.js
export default Ember.Component.extend({
  actions: {
    click: function(){
      this.sendAction('on-click');
    }
  }
});
```

```handlebars
<!-- my-control.hbs -->
{{my-button on-click="submit"}}
```

---

# Limitations : imbrication pénible

```javascript
// app/index/my-control.js
export default Ember.Component.extend({
  actions: {
    submit: function(){
      this.sendAction('submit');
    }
  }
});
```

```handlebars
<!-- my-form.hbs -->
{{my-control submit="submit"}}
```

---

# Limitations : imbrication pénible

```javascript
// app/index/my-form.js
export default Ember.Component.extend({
  actions: {
    submit: function(){
      this.sendAction('submit');
    }
  }
});
```

```handlebars
<!-- template.hbs -->
{{my-form submit="save"}}
```

---

# Limitations : imbrication pénible

```javascript
// controller.js
export default Ember.Controller.extend({
  actions: {
    save: function(){
      return 'great success';
    }
  }
});
```

---

.small[

```javascript
// app/index/my-button.js
export default Ember.Component.extend({
  actions: {
    click: function(){
      this.sendAction('on-click');
    }
  }
});
```

```handlebars
<!-- my-control.hbs -->
{{my-button on-click="submit"}}
```

```javascript
// app/index/my-button.js
export default Ember.Component.extend({
  actions: {
    click: function(){
      this.sendAction('on-click');
    }
  }
});
```

```handlebars
<!-- my-control.hbs -->
{{my-button on-click="submit"}}
```

```javascript
// app/index/my-form.js
export default Ember.Component.extend({
  actions: {
    submit: function(){
      this.sendAction('submit');
    }
  }
});
```

```handlebars
<!-- template.hbs -->
{{my-form submit="save"}}
```

```javascript
// controller.js
export default Ember.Controller.extend({
  actions: {
    save: function(){
      return 'great success';
    }
  }
});
```
]

---

# Alors qu'on voudrait juste faire

.center[
```handlebars
{{my-form}} ← {{my-control}} ← {{my-button}}
```
]

---

class: center, middle
# Ember Improved Actions 🙌

---

# Passer une closure plutôt qu’une string

--

```handlebars
<!-- template.hbs -->
{{my-form submit=(action "save")}}
```

--

```javascript
// my-form.js
export default Ember.Component.extend({
  click: function(){
    // this.sendAction('submit'); // submit action, legacy
    this.attrs.submit(); // submit action, new style
  }
});
```

---

# Imbriquer les actions facilement

```handlebars
<!-- template.hbs -->
{{my-form submit=(action "save")}}
```

```handlebars
<!--  my-form.hbs -->
{{my-control submit=attrs.submit}}
```

```handlebars
<!-- my-control.hbs -->
{{my-button on-click=attrs.submit}}
```

```javascript
// my-button.js
export default Ember.Component.extend({
  click: function(){
    // this.sendAction('on-click'); // submit action, legacy
    this.attrs['on-click'](); // submit action, new style
  }
});
```

---

# Renvoyer une valeur

```javascript
// controller.js
export default Ember.Controller.extend({
  actions: {
    submit: function(){
      return 'great success';
    }
  }
});
```

--

```javascript
// my-button.js
export default Ember.Component.extend({
  click: function(){
    var result = this.attrs['on-click']();
    result; // => 'great success'
  }
});
```

---

# Agenda

<br><br>

.big[
- 6 mai : RFC
]

--

.big[
- 9 mai : Pull Request
]

--

.big[
- 10 mai : Pull Request mergée
]

--

.big[
- 11 mai : activé par défaut
]

---

class: center, middle
# Disponible dans Ember 1.13 beta.

---

class: center, middle

# ✨
