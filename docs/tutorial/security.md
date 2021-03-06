# Security, Native Capabilities, and Your Responsibility

As web developers, we usually enjoy the strong security net of the browser - the
risks associated with the code we write is relatively small. We rely on the
fairly limited amount of power and capabilities granted to a website – and trust
that our users enjoy a browser built by a large team of engineers that is able
to quickly respond to newly discovered security threats.

When working with Electron, it is important to understand that Electron isnot a
web browser. It allows you to build powerful desktop apps with web technologies.
Its core feature is the ability to build software is just as powerful as
completely native applications, eclipsing the limited feature set of a website.
The inherent risks scale with the additional powers granted to your code.

With that in mind, be aware that displaying arbitrary content from untrusted
sources poses a severe security risk that Electron is not built to handle.
In fact, the most popular Electron apps (Atom, Slack, Visual Studio Code, etc)
display primarily local content (or trusted, secure remote content without Node
integration) – if your application executes code from an online source, it is
your responsibility to ensure that the code is not malicious.

## Chromium Security Issues and Upgrades

While Electron strives to support new versions of Chromium as soon as possible,
developers should be aware that upgrading is a serious undertaking - involving
hand-editing dozens or even hundreds of files. Given the resources and
contributions available today, Electron will often not be on the very latest
version of Chromium, lagging behind by either days or weeks.

We feel that our current system of updating the Chromium component strikes an
appropriate balance between the resources we have available and the needs of the
majority of applications built on top of the framework. We definitely are
interested in hearing more about specific use cases from the people that build
things on top of Electron. Pull requests and contributions supporting this
effort are always very welcome.

## Ignoring Above Advice
A security issue exists whenever you receive code from a remote destination and
execute it locally. As an example, consider a remote website being displayed
inside a browser window. If an attacker somehow manages to change said content
(either by attacking the source directly, or by sitting between your app and
the actual destination), they will be able to execute native code on the user's
machine.

> :warning: Under no circumstances should you load and execute remote code with
enabled Node integration. Instead, use only local files (packaged together with
your application) to execute Node code. To display remote content, use the
`webview` tag and make sure to disable the `nodeIntegration`.

#### Checklist
This is not bulletproof, but at the least, you should attempt the following:

* Only display secure (https) content
* Disable the Node integration in all renderers that display remote content
  (using `webPreferences`)
* Do not disable `webSecurity`. Disabling it will disable the same-origin policy.
* Do not set `allowDisplayingInsecureContent` to true.
* Do not set `allowRunningInsecureContent` to true.
* Do not enable `experimentalFeatures` or `experimentalCanvasFeatures` unless
  you know what you're doing.
* Do not use `blinkFeatures` unless you know what you're doing.
* WebViews: Set `nodeintegration` to false
* WebViews: Do not use `disablewebsecurity`
* WebViews: Do not use `allowpopups`
* WebViews: Do not use `insertCSS` or `executeJavaScript` with remote CSS/JS.

Again, this list merely minimizes the risk, it does not remove it. If your goal
is to display a website, a browser will be a more secure option.
