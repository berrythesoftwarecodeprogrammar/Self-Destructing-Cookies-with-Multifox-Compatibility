# Self Destructing Cookies (with Multifox Compatibility)
This is just the [Self-Destructing Cookies](https://addons.mozilla.org/en-US/firefox/addon/self-destructing-cookies/) addon for Firefox, modified to not delete cookies belonging to [Multifox](https://addons.mozilla.org/en-US/firefox/addon/multifox/) tabs.

Download signed `.xpi`s at [Releases page](https://github.com/berrythesoftwarecodeprogrammar/Self-Destructing-Cookies-Multifox-patch/releases)  
**IT IS NOW SIGNED THANKS TO SDC USING JPM NOW :D**
Download latest (0.4.101) [🡇](https://github.com/berrythesoftwarecodeprogrammar/Self-Destructing-Cookies-with-Multifox-Support/releases/tag/0.4.101)

All code belongs to [Ove](https://addons.mozilla.org/en-US/firefox/user/ovso/) (sdc@elektro-eel.org) except for the single line I added lol

## Info

Really just requires adding a single line to a function in `main.js`

```javascript
function checkDomainWhitelist(domain, ignoreSession) {
  // an empty domain indicates a file:// type url, keep those cookies
  if (domain == "") return true;
  if (domain.substr(-9) === ".multifox") { arr = domain.split("."); arr.splice(-2,2); domain = arr.join("."); } //brr
  // XXX assuming that subdomains are automatically whitelisted
  var perm = getDomainWhitelist(domain);
  return (perm == Ci.nsICookiePermission.ACCESS_ALLOW || perm == Ci.nsICookiePermission.ACCESS_ALLOW_FIRST_PARTY_ONLY || (!ignoreSession && perm == Ci.nsICookiePermission.ACCESS_SESSION));
}
```

the line added is:
```javascript
if (domain.substr(-9) === ".multifox") { arr = domain.split("."); arr.splice(-2,2); domain = arr.join("."); } //brr
```

I know it's not perfect. If a tab is open with cookie `github.com.github-com-2.multifox` and a tab is open with cookie `github.com` and the multifox tab is closed, it's cookies will remain as long as the non-multifox github.com tab is open as all the modification does is strip the multifox suffix when checking cookies agains the whitelist of open tabs. But at least it makes browsing with Multifox+SDC not annoying by not deleting cookies which are being used.

## Modifying the extension yourself

This is recommended if you don't trust me. All I am doing is uploading the contents of another person's extension with a single line changed which you can do yourself. 

1. Visit the [Self-destructing Cookies addon page](https://addons.mozilla.org/en-US/firefox/addon/self-destructing-cookies/)
2. Right click on the install button and Save As
3. Extract the .xpi contents with an archive manager (its just a zip)
4. Add the line mentioned above to `\resources\self-destructing-cookies\lib\main.js`
5. Delete the `META-INF` folder
6. Edit `package.json` and `install.rdf` to reflect your new addon ID, title and version
7. `jpm xpi`
8. `jpm sign --api-key (your key) --api-secret (your secret) --xpi (your xpi)`
9. Drag signed .xpi into firefox

## License 
[GNU General Public License, version 2.0](http://www.gnu.org/licenses/gpl-2.0.html) (Original license of SDC)
