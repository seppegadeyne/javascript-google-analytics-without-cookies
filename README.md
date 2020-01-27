# Google Analytics without cookies

Google Analytics without cookies, anonymized ip addresses and anonymized clientId.

Fingerprintjs2 is used for the clientId, https://github.com/Valve/fingerprintjs2

```javascript
<!-- Google Analytics anonymous and without cookies -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/fingerprintjs2/2.1.0/fingerprint2.min.js"></script>
<script>
    (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
        (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
        m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
    })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

    function gaCreate() {
        Fingerprint2.get(function (components) {
            let values = components.map(function (component) { return component.value });
            let murmur = Fingerprint2.x64hash128(values.join(''), 31);
            // Replace XX-XXXXXXXXX-X with your unique GA tracking code
            ga('create', 'XX-XXXXXXXXX-X', {
                'storage':'none',
                'clientId':murmur
            });
            ga('set', 'allowAdFeatures', false);
            ga('set', 'anonymizeIp', true);
            ga('send', 'pageview');
        })
    }

    if (window.requestIdleCallback) {
        requestIdleCallback(function () {
            gaCreate();
        })
    } else {
        setTimeout(function () {
            gaCreate();
        }, 500)
    }
</script>
```
