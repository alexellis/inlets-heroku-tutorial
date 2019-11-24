# inlets-heroku-tutorial

Run an [inlets](https://inlets.dev/) server on Heroku

## Push a container image

* Log into your dahsboard, create an app and get the CLI:

https://dashboard.heroku.com

* Login:

```
heroku container:login
```

* Deploy:

```sh
heroku container:push web -a my-inlets
heroku container:release web -a my-inlets
heroku restart web -a my-inlets
```

* Set auth token

```sh
export TOKEN=$(head -c 16 /dev/urandom | shasum | cut -f1 -d"-")
echo "Token is: ${TOKEN}"

export HEROKU_APP=my-inlets

heroku config:set TOKEN=${TOKEN} --app $HEROKU_APP
echo $TOKEN > token
```

* Run a test server

```sh
mkdir -p /tmp/inlets
cd /tmp/inlets
uname > README.md
python -m SimpleHTTPServer
```

* Connect the `inlets client`

```sh
export HEROKU_APP="my-inlets"

inlets client \
  --remote wss://${HEROKU_APP}.herokuapp.com \
  --token $(cat token) \
  --upstream http://127.0.0.1:8000
```

* Now test the tunnel:

```
https://${HEROKU_APP}.herokuapp.com/
```

