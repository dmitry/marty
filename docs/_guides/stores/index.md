---
layout: page
title: Stores
id: stores
section: Stores
---

The store is where your state should live. It is also the only place that your state should **change**. Actions come in from the dispatcher and the store  decides whether to handle them of not. If the store does choose to handle an action and subsequently changes its state then it will notify any listeners that it has changed. Your views can then listen to those changes (via the [State Mixin](/guides/state-mixin)) and then re-render themselves.

All of a stores state should live within ``Store#state``. If you want to update the state you should use ``Store#setState`` which will update ``this.state`` and then notify any listeners that the store has changed. Or if you prefer you can update ``this.state`` and then manually call ``this.hasChanged()``

When you create a store it will automatically start listening to the dispatcher. To determine which actions to handle you define the [handlers](/api/stores/#handlers) hash. The keys in handlers is the function you wish to call when an action comes in and the value is an [action predicates](/api/stores/#action-predicates) (Normally the constant of the action's type). If your store does handle the action, then the arguments you passed to [ActionCreator#dispatch](/api/action-creators/index.html#dispatch) are the arguments for the handler.

{% highlight js %}
var UsersStore = Marty.createStore({
  name: 'Users',
  handlers: {
    addUser: Constants.RECEIVE_USER
  },
  getInitialState: function () {
    return {};
  },
  addUser: function (user) {
    this.state[user.id] = user;
    this.hasChanged();
  }
});

var listener = UsersStore.addChangeListener(function () {
  console.log('Users store changed');
  listener.dispose();
});
{% endhighlight %}