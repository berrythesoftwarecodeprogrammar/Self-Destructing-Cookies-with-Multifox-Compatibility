# Self Destructing Cookies (Multifox patch)
[Self-Destructing Cookies Firefox Addon](https://addons.mozilla.org/en-US/firefox/addon/self-destructing-cookies/) modified to not delete cookies belonging to [Multifox](https://addons.mozilla.org/en-US/firefox/addon/multifox/) tabs.

Really just requires adding a single line to a function in `main.js`

ALL CODE belongs to [Ove](https://addons.mozilla.org/en-US/firefox/user/ovso/) (sdc@elektro-eel.org) except for the single line I added lol

```
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
```
if (domain.substr(-9) === ".multifox") { arr = domain.split("."); arr.splice(-2,2); domain = arr.join("."); } //brr
```
