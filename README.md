# Reapop
[![npm version](https://img.shields.io/npm/v/reapop.svg?style=flat-square)](https://www.npmjs.com/package/reapop) [![npm dependencies](https://img.shields.io/david/LouisBarranqueiro/reapop.svg?style=flat-square)](https://www.npmjs.com/package/reapop) [![npm dependencies](https://img.shields.io/david/dev/LouisBarranqueiro/reapop.svg?style=flat-square)](https://www.npmjs.com/package/reapop) [![travis build status](https://img.shields.io/travis/LouisBarranqueiro/reapop/master.svg?style=flat-square)](https://travis-ci.org/LouisBarranqueiro/reapop) [![coveralls status](https://img.shields.io/coveralls/LouisBarranqueiro/reapop.svg?style=flat-square)](https://coveralls.io/github/LouisBarranqueiro/reapop) [![npm download/month](https://img.shields.io/npm/dm/reapop.svg?style=flat-square)](https://www.npmjs.com/package/reapop) [![gitter chat](https://img.shields.io/gitter/room/LouisBarranqueiro/reapop.svg?style=flat-square)](https://gitter.im/LouisBarranqueiro/reapop)
  
A React and Redux notifications system

## Summary

* [Compatibility](https://github.com/LouisBarranqueiro/reapop#compatiblity)
* [Demo](https://github.com/LouisBarranqueiro/reapop#demo)
* [Installation](https://github.com/LouisBarranqueiro/reapop#installation)
* [Integration](https://github.com/LouisBarranqueiro/reapop#integration)
* [Usage](https://github.com/LouisBarranqueiro/reapop#usage)
    * [In a React component](https://github.com/LouisBarranqueiro/reapop#in-a-react-component)
    * [In a Redux async action creator](https://github.com/LouisBarranqueiro/reapop#in-a-redux-async-action-creator)
* [API documentation](https://github.com/LouisBarranqueiro/reapop#api-documentation)
    * [Customize style and behavior](https://github.com/LouisBarranqueiro/reapop#customize-style-and-behavior)
    * [Add a notification](https://github.com/LouisBarranqueiro/reapop#add-a-notification)
    * [Update a notification](https://github.com/LouisBarranqueiro/reapop#update-a-notification)
    * [Remove a notification](https://github.com/LouisBarranqueiro/reapop#remove-a-notification)

## Compatibility

### Library supports

Tested and works with :

- [react](https://github.com/facebook/react) : **^0.14.0** and **^15.0.0**
- [react-redux](https://github.com/reactjs/react-redux) : **^2.0.0** and **^3.0.0** and **^4.0.0**
- [redux](https://github.com/reactjs/redux) : **^2.0.0** and **^3.0.0**

### Browsers supports

Tested and works with :

- Chrome : **50**
- Firefox : **46**
- Safari : **9**

## Demo

Check out the [demo](http://louisbarranqueiro.github.io/reapop/)

## Installation

```
npm install --save reapop
```

## Integration

Render this component at the root of your web application to avoid position conflicts.

``` js
import React, {Component} from 'react';
import NotificationsSystem from 'reapop';

class ATopLevelComponent extends Component {
  render() { 
    return (
      <div>
        <NotificationsSystem/>
      </div>
    );
  }
}
```

Apply `thunk` middleware from [redux-thunk](https://github.com/gaearon/redux-thunk) to your Redux store.

``` js
import {createStore, compose, applyMiddleware} from 'redux';
import thunk from 'redux-thunk';

// store
const createStoreWithMiddleware = compose(
  applyMiddleware(thunk)
)(createStore);
const store = createStoreWithMiddleware(combineReducers({
    // your reducers here
  }), {});

```

## Usage

### In a React component

If you are not familiar with react-redux library or the way to connect a React component with a Redux store, I recommend you to read [Redux documentation - Usage with React](http://redux.js.org/docs/advanced/UsageWithReact.html) to understand this example.

``` js
import React, {Component} from 'react';
import {connect} from 'react-redux';
// 1. we import `addNotification() as notify()` (async action creator)
import {addNotification as notify} from 'reapop';

class AmazingComponent extends Component {
  constructor(props) {
    super(props);
    // 4. don't forget to bind method
    this._onClick = this._onClick.bind(this);
  }

  _onClick() {
    const {notify} = this.props;
    // 3. we use `notify` to create a notification 
    notify({
      title: 'Welcome',
      message: 'you clicked on the button',
      status: 'success',
      dismissible: true,
      dismissAfter: 3000
    });
  }

  render() {
    return (
      <div>
        // 5. we notify user when he click on the button
        <button onClick={this._onClick}>Add a notification</button>
      </div>
    );
  }
}
// 2. we map dispatch to props `notify` async action creator
//    here we use a shortcut instead of passing a `mapDispathToProps` function
export default connect(null, {notify})(AmazingComponent);
```

### In a Redux async action creator

If you are not familiar with async actions creator, I recommend you to read [Redux documentation - Async actions](http://redux.js.org/docs/advanced/AsyncActions.html) to understand this example.
``` js
// 1. we import `addNotification() as notify()` (async action creator) 
import {addNotification as notify} from 'reapop';

// we add a notification to inform user about
// state of his request (success or failure) 
const sendResetPasswordLink = (props) => {
  return (dispatch) => {
    axios.post('users/ask-reset-password', props)
      .then((res) => {
        // 2. we use `dispatch()` to notify user
        // status code will be converted in an understandable status for the Component
        dispatch(notify({message:res.data.detail, status:res.statusCode}));
      })
      .catch((res) => {
       // 3. same thing here
        dispatch(notify({message:res.data.detail, status:res.statusCode}));
      });
    };
};
```

## API documentation

### Customize style and behavior

Here is the list of the things which are customizable by setting properties of the `NotificationsSystem` component :

| Property              | Type   | Description |
| --------------------- | :----: | ----------- |
| className             | String | Class names of `NotificationsSystem` component. Check [className](https://github.com/LouisBarranqueiro/reapop#classname-property) property |
| config                | Object | Config of `NotificationsSystem` component. Check [config](https://github.com/LouisBarranqueiro/reapop#config-property) property |
| containerClassName    | Object | Class names of `NotificationsContainer` component. Check [containerClassName](https://github.com/LouisBarranqueiro/reapop#containerclassName-property) property |
| defaultValues         | Object | Default value for a notification. Check [defaultValues](https://github.com/LouisBarranqueiro/reapop#defaultvalues-property) property |
| notificationClassName | Object | Class names of `Notification` component. Check [notificationClassName](https://github.com/LouisBarranqueiro/reapop#notificationclassname-property) property |
| transition            | Object | Transition for `Notification` component. Check [transition](https://github.com/LouisBarranqueiro/reapop#transition-property) property |

#### `className` properties

It allow you to configure class names of `NotificationsSystem` component.
By editing this property, you can change style of notifications containers. 
Of course, you will have to write your own CSS. 

##### JSX structure of `NotificationsSystem` React component

``` html
<div className={className}>
    <!-- childrens (NotificationsContainer) -->
</div>
``` 
##### Example

``` js 
import React, {Component} from 'react';
import NotificationsSystem from 'react';

class AComponent extends Component {
  render() {
    const defaultValues = {
      status: STATUS.default,
      position: POSITIONS.topRight,
      dismissible: true,
      dismissAfter: 5000,
      allowHTML: false
    };
    return (<NotificationsSystem className="my-custom-classname" />);
  }
}
```

#### `config` property

This object allow you to configure behavior of `NotificationsSystem` component

| Property       | Type    | Default | Description |
| -------------- | :-----: | :-----: | ----------- |
| smallScreenMin | Number  | 768     | Minimal width for small screen (min-width). This value is used to determine when all notifications are rendered at the top of the screen what ever the position property of a notification object |

##### Example

``` js 
import React, {Component} from 'react';
import NotificationsSystem from 'react';

class AComponent extends Component {
  render() {
    const config = {
      smallScreenMin: 768
    };
    return (
      <NotificationsSystem config={config} />
    );
  }
}
```

#### `containerClassName` properties

This object allow you to configure class names of `NotificationsContainer` component.
By editing this property, you can change style of notifications containers. 
Of course, you will have to write your own CSS. 

| Property    | Type     | Description |
| ----------- | :------: | ----------- |
| main        | String   | Applied on root of `NotificationContainer` component. |
| position    | Function | Applied on root of `NotificationContainer` component. Use to stylize component depending on its position. |

##### JSX structure of `NotificationsContainer` component

``` html
<div className={`${className.main} ${className.position(position)}`}>
  <!-- childrens (Notification) -->
</div>
```

##### Example

``` js
import React, {Component} from 'react';
import NotificationsSystem from 'react';

class AComponent extends Component {
  render() {
    // here is a complete example
    const containerClassName = {
      main: css['notifications-container'],
      position: function(position) {
        return css[`notifications-container--${position}`];
      }
    };
    return (<Notifications containerClassName={containerClassName}/>);
  }
}
```

#### `defaultValues` property

This object allow you to configure default behavior for your notifications.

| Property     | Type    | Default | Description |
| ------------ | :-----: | :-----: | ----------- |
| status       | String  | null    | Status of the notification : info, success, warning, error |
| position     | String  | tr      | Position of the  notification : tl, tr, br, bl |
| dismissible  | Boolean | true    | Define if the notification is dismissible by clicking on it |
| dismissAfter | Number  | 5000    | Time before the notification disappear (ms). 0: infinite |
| allowHTML    | Boolean | False   | Allow you to insert HTML in the notification. Read [this](https://facebook.github.io/react/tips/dangerously-set-inner-html.html) before setting this value to true. |

##### Example

``` js 
import React, {Component} from 'react';
import NotificationsSystem, {STATUS, POSITIONS} from 'reapop';

class AComponent extends Component {
  render() {
    const defaultValues = {
      status: STATUS.default,
      position: POSITIONS.topRight,
      dismissible: true,
      dismissAfter: 5000,
      allowHTML: false
    };
    return (
      <NotificationsSystem config={config} />
    );
  }
}
```

#### `transition` properties

This object allow you to configure CSS animation of `Notification` component. 
We use React High-level API : [ReactCSSTransitionGroup](https://facebook.github.io/react/docs/animation.html#high-level-api-reactcsstransitiongroup) to animate notifications.
By editing this property, you can change the default transition. 
Of course, you have to write your own animation in CSS. 
I recommend you to use the [initial code](https://github.com/LouisBarranqueiro/reapop/blob/master/src/components/NotificationsContainer/styles.scss) as a base.

| Property     | Type    | Default | Description |
| ------------ | :-----: | :-----: | ----------- |
| enterTimeout | Number  | 400     | Duration of enter animation (ms) |
| leaveTimeout | Number  | 400     | Duration of leave animation (ms) |
| name         | Object  | Object  | Classes to trigger a CSS animation or transition |

##### Example

``` js
import React, {Component} from 'react';
import NotificationsSystem from 'react';

class AComponent extends Component {
  render() {
    const transition = {
      enterTimeout: 400,
      leaveTimeout: 400,
      name: {
        enter: 'enter',
        enterActive: 'enterActive',
        leave: 'leave',
        leaveActive: 'leaveActive'
      }
    };
    return (<Notifications transition={transition}/>);
  }
}
```

#### `notificationClassName` properties

This object allow you to configure class names of `Notification` component.

| Property    | Type     | Description |
| ----------- | :------: | ----------- |
| main        | String   | Applied on root notification container. |
| meta        | String   | Applied on notification meta container. |
| title       | String   | Applied on notification title container. |
| message     | String   | Applied on notification message container. |
| icon        | String   | Applied on notification icon container. |
| status      | Function | Applied on root notification container. Use to stylize the notification depending on its status. |
| dismissible | String   | Applied on notification dismissible container. |
| buttons     | Function | Applied on root notification container to stylize the notification depending on number of buttons it has; and on notification buttons container. |
| button      | String   | Applied on notification button container. |
| buttonText  | String   | Applied on container of text of notification button. |

##### JSX structure of `Notification` component

``` html
<div class={`${className.main} ${className.status(status)} 
  ${className.buttons(buttons.length) ${(isDismissible ? className.dismissible : '')}`}>
  <i class="${className.icon}"></i>
  <div class="{className.meta}">
    <h4 class="{className.title}">Est at quibusdam nisi ex.</h4>
    <p class="{className.message}">Veritatis eum impedit molestiae aut.</p>
  </div>
  <div class={className.buttons()}>
    <button className={className.button}>
      <span className={className.buttonText}>
        <b>Yes</b>
      </span>
    </button>
    <button className={className.button}>
      <span className={className.buttonText}>
        <b>No</b>
      </span>
    </button>
  </div>
</div>
```

##### Example

``` js
import React, {Component} from 'react';
import NotificationsSystem from 'react';

class AComponent extends Component {
  render() {
    // here is a complete example
    const className = {
      main: css['notification'],
      metameta: css['notification-meta'],
      title: css['notification-title'],
      message: css['notification-message'],
      icon: `fa ${css['notification-icon']}`,
      status: (status) => {
        return css[`notification--${status}`];
      },
      dismissible: css['notification--dismissible'],
      // `fa` corresponds to font-awesome's class name
      buttons: (count) => {
        if (count === 0) {
          return '';
        }
        else if (count === 1) {
          return css['notification--buttons-1'];
        }
        else if (count === 2) {
          return css['notification--buttons-2'];
        }
        return css['notification-buttons'];
      },
      button: css['notification-button'],
      buttonText: css['notification-button-text']
    };
    return (<Notifications notificationClassName={notificationClassName}/>);
  }
}
```

### Add a notification

Adding notification is done with the `addNotification` (async action creator) function. It returns the notification object just added.

#### Syntax

``` js
addNotification(notification);
```

#### Parameters

| Parameter    | Type     | Description |
| ------------ | :------: | ----------- |
| notification | Object   | A [notification](https://github.com/LouisBarranqueiro/reapop#notification-object-properties) object |

#### Notification object properties
 
| Property     | Type             | Default | Description |
| ------------ | :--------------: | :-----: | ----------- |
| title        | String           |         | Title of the notification |
| message      | String           |         | Message of the notification |
| status       | String or Number | default | Status of the notification : default, info, success, warning, error. You can also pass an HTTP status code like 200, or 403, it will be converted as an understandable status for the `Notification` component |
| position     | String           | tr      | Position of the notification on the screen |
| dismissible  | Boolean          | true    | Define if a notification is dismissible by clicking on it |
| dismissAfter | Number           | 5000    | Time before the notification disappear (ms). Paused when mouse is hovering the notification. 0: infinite. |
| buttons      | Array            |         | Array of [button](https://github.com/LouisBarranqueiro/reapop#button-object-properties) objects. A notification can have 2 buttons maximum. |
| onAdd        | Function         |         | Function executed at component lifecycle : `componentDidMount` |
| onRemove     | Function         |         | Function executed at component lifecycle : `componentWillUnmount` |
| allowHTML    | Boolean          | false   | Allow HTML in message |

##### Button object properties
 
| Property     | Type     | Default | Description |
| ------------ | :------: | :-----: | ----------- |
| name         | String   |         | Title of the button |
| primary      | Boolean  | false   | true: Title in bold, false : title in normal |
| onClick      | Function |         | Function executed when user click on it |

#### Example

``` js
const notif = addNotification({
  title: 'Welcome on demo!',
  message: 'Hey buddy, here you can see what you can do with it.',
  position: 'br',
  status: 'info',
  dismissAfter: 10000,
  dismissible: false,
  onAdd: function() {
    console.log('hey buddy');
  },
  onRemove: function() {
      console.log('cya buddy');
  },
  buttons:[{
    name: 'OK',
    primary:true,
    onClick: () => {
      console.log('i\'m OK too');
    }
  }] 
});
console.log(JSON.stringify(notif));
/*
{
  "id":1463345312016,
  "title":"Welcome on demo!",
  "message":"Hey buddy, here you can see what you can do with it.",
  "position":"br",
  "status":"info",
  "dismissAfter":10000,
  "dismissible":false,
  "buttons":[{
    "name":"OK",
    "primary":true
  }]
}
*/

```

### Update a notification

Updating a notification is done with the `updateNotification` (action) function.

#### Syntax

``` js
updateNotification(notification);
```

#### Parameters

| Parameter    | Type     | Description |
| ------------ | :------: | ----------- |
| notification | Object   | A [notification](https://github.com/LouisBarranqueiro/reapop#notification-object-properties-1) object |

#### Notification object properties
 
| Property     | Type             | Default | Description |
| ------------ | :--------------: | :-----: | ----------- |
| title        | String           |         | Title of the notification |
| message      | String           |         | Message of the notification |
| status       | String or Number | default | Status of the notification : default, info, success, warning, error. You can also pass an HTTP status code like 200, or 403, it will be converted as an understandable status for the `Notification` component |
| position     | String           | tr      | Position of the notification on the screen |
| dismissible  | Boolean          | true    | Define if a notification is dismissible by clicking on it |
| dismissAfter | Number           | 5000    | Time before the notification disappear (ms). Paused when mouse is hovering the notification. 0: infinite. |
| buttons      | Array            |         | Array of [button](https://github.com/LouisBarranqueiro/reapop#button-object-properties-1) object. A notification can have 2 buttons maximum. |
| onAdd        | Function         |         | Function executed at component lifecycle : `componentDidMount` |
| onRemove     | Function         |         | Function executed at component lifecycle : `componentWillUnmount` |
| allowHTML    | Boolean          | false   | Allow HTML in message |

##### Button object properties
 
| Property     | Type     | Default | Description |
| ------------ | :------: | :-----: | ----------- |
| name         | String   |         | Title of the button |
| primary      | Boolean  | false   | true: Title in bold, false : title in normal |
| onClick      | Function |         | Function executed when user click on it |


#### Example

``` js
let notif = addNotification({
  title: 'Upload status',
  message: 'Your file is uploading...',
  status: 'info',
  dismissible: false,
  dismissAfter: 0
});
// simulate file upload
setTimeout(function() {
  notif.status = 'success';
  notif.message = 'Your file has been successfully uploaded';
  notif.dismissible = true;
  notif.dismissAfter = 5000;
  updateNotification(notif);
}, 10000);
```


### Remove a notification

Removing a notification is done with `removeNotification` (action) function.

#### Syntax

``` js
removeNotification(id);
```

#### Parameters

| Parameter   | Type   | Description |
| ----------- | :----: | ----------- |
| id          | Number | id of the notification |

# License 

Reapop is under [MIT License](https://github.com/LouisBarranqueiro/reapop/blob/master/LICENSE)