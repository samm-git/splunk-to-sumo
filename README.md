# splunk-to-sumo

This tool is intended to use the splunk docker logging driver to emit docker logs directly to sumologic.
It is a reverse proxy that is typically configured side-by-side on a docker worker host.

## Run

Running the proxy server is super simple, just specify the bind address as the first argument.
If not specified, will default to :5001

```
$ ./splunk-to-sumo 0.0.0.0:5001
```

## Configure docker logging

When running a docker container, configure to use splunk logging driver.
Note that `tag` is optional and is packaged in the final message body.
`splunk-token` is used to set the collector endpoint.
The `name` and `category` querystring parameters are transmuted into `X-Sumo-Host` and `X-Sumo-Category` headers.
The `<collector code>` is the code unique to the source.

```
$ docker run \
  --log-driver=splunk \
  --log-opt tag="{{.Name}}/{{.FullID}}" \
  --log-opt splunk-url=http://<bind address> \
  --log-opt splunk-token=https://endpoint1.collection.us2.sumologic.com/receiver/v1/http/<collector code>?name=<name>&category=<category> \
  ubuntu echo "Hello world"
```
