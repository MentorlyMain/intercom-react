# intercom-react

[![CircleCI](https://circleci.com/gh/kvendrik/intercom-react.svg?style=svg)](https://circleci.com/gh/kvendrik/intercom-react)
[![Coverage Status](https://coveralls.io/repos/github/kvendrik/intercom-react/badge.svg?branch=master)](https://coveralls.io/github/kvendrik/intercom-react?branch=master)
![Bundle Size](https://img.shields.io/badge/gzip%20size-3.42%20kB-44cc11.svg)
[![NPM Version](https://badge.fury.io/js/intercom-react.svg)](https://yarnpkg.com/en/package/intercom-react)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An [Intercom](http://intercom.com/) component for React that truly encapsulates the library and makes it work like a "real" React component.

## Installation

```
yarn add intercom-react
```

_️️⚠️ Before you install the library make sure to read the [notes on reliability](#️-a-few-notes-on-reliability)._

## Setup

```tsx
<Intercom
  open
  appId="fyq3wodw"
  languageOverride="en"
  user={{
    user_id: '9876',
    email: 'john.doe@example.com',
    created_at: 1234567890,
    name: 'John Doe',
  }}
  onOpen={() => {}}
  onClose={() => {}}
  onUnreadCountChange={(unreadCount) => {}}
  onInitialization={(intercom) => {}}
/>
```

- `appId`: the ID of your app.
- `user` (optional): all user data. If this changes during the lifecycle the component will call `intercom('update', userData)`.
- `languageOverride` (optional): the language to display content in; overrides the browser default.
- `open` (optional): whether Intercom is showing or not.
- `onOpen` (optional): called when intercom opens.
- `onClose` (optional): called when intercom closes.
- `onUnreadCountChange` (optional): called when the unread count changes.
- `onInitialization` (optional): called when intercom has initialized. The component passes the `intercom` method to this callback in case you require advanced usage like [emitting events](https://developers.intercom.com/docs/intercom-javascript#section-intercomtrackevent) or [pre-populating content](https://developers.intercom.com/docs/intercom-javascript#section-intercomshownewmessage).
- `launcher` (optional): whether a launcher button should be shown. Defaults to `true`.

## How is this different?

Intercom is the support tool of choice for myself and the companies I work for (because its an amazing tool!). An issue I have always had with their library is that its not particularly React friendly. There is a set of React Intercom libraries out there but most of them just mount Intercom to the global scope.

What this means is that if you would get into a situation where you would have to unmount Intercom the three DOM nodes the library mounts (`#intercom-container`, `#intercom-stylesheet` and `#intercom-frame`) will stay mounted, as will the four event listeners the library mounts (2x `beforeunload` and 2x `message`). In other words the existing components are not unmountable.

I wrote this component to create an isolated Intercom component that cleans up after itself when unmounted for a "true" React experience.

## ⚠️ A few notes on reliability

The main purpose of this component is to provide a way for you to integrate Intercom into your project without having it live in the global scope and it therefor being unmountable.

Getting that to work took quite a bit of [reverse engineering](https://github.com/kvendrik/intercom-react/pull/15) and I haven't been able to find a way to include a specific version of the library yet which means that **things might stop working as new versions of the Intercom library release** and that **the library should therefor be viewed as experimental**.

I would therefor recommend that you only use this library if you have a very solid reason for needing Intercom to be unmountable. If not I recommend you use a solution like [`react-intercom`](https://github.com/nhagen/react-intercom) which simply mounts Intercom to the global scope.

Having that said I appreciate your interest in the library and look forward to hearing about your experience with it 🙌 .

## 🏗 Contributing

1.  Make your changes.
2.  Check your changes in the playground (`yarn playground`).
3.  Build using `yarn build` and change your playground `Intercom` import to the build by changing the path to `../`.
4.  Test you changes in/on multiple browsers and devices.
5.  Add/Alter the appropriate tests.
6.  Make sure all tests pass (`yarn lint && yarn test`).
7.  Create a PR.
