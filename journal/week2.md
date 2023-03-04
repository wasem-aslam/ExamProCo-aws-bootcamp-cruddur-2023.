## Week 2 — Distributed Tracing


# Technical Tasks


✅ Instrument our backend flask application to use Open Telemetry (OTEL) with Honeycomb.io as the provider.

✅ Run queries to explore traces within Honeycomb.io

✅ Instrument AWS X-Ray into backend flask application

✅ Configure and provision X-Ray daemon within docker-compose and send data back to X-Ray API

✅ Observe X-Ray traces within the AWS Console

✅ Integrate Rollbar for Error Logging

✅ Trigger an error an observe an error with Rollbar

✅ Install WatchTower and write a custom logger to send application log data to - CloudWatch Log group
 
 
 #  Homework Challenges 
    
✅ Instrument Honeycomb for the frontend-application to observe network latency between frontend and backend[HARD]

✅ Add custom instrumentation to Honeycomb to add more attributes eg. UserId, Add a custom span

✅ Run custom queries in Honeycomb and save them later eg. Latency by UserID, Recent Traces

######################################################################################################

✅ Instrument our backend flask application to use Open Telemetry (OTEL) with Honeycomb.io as the provider.

First created account in https://ui.honeycomb.io/ and followed the [GuideLine](https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-2/journal/week2.md) 


Added following code in requirements.txt

```
opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests
```

Installed above dependencies.

![](assets/week2/Requirements%20installed%20.png)


Updated app.py with following code.

```
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
```

```
# Initialize tracing and an exporter that can send data to Honeycomb
provider = TracerProvider()
processor = BatchSpanProcessor(OTLPSpanExporter())
provider.add_span_processor(processor)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer(__name__)
```

```
# Initialize automatic instrumentation with Flask
app = Flask(__name__)
FlaskInstrumentor().instrument_app(app)
RequestsInstrumentor().instrument()
```

Also added Environment variables to docker-compose file.

```
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
OTEL_SERVICE_NAME: "${HONEYCOMB_SERVICE_NAME}"
```

I also added the environment variables with keys and service name.

```
export HONEYCOMB_API_KEY=""
export HONEYCOMB_SERVICE_NAME="Cruddur"
gp env HONEYCOMB_API_KEY=""
gp env HONEYCOMB_SERVICE_NAME="Cruddur"
```


I performed few operations at port 3000 and 4567.

https://4567-wasemaslam-awsbootcampc-6zu1jja30gr.ws-eu89.gitpod.io/api/activities/home

https://4567-wasemaslam-awsbootcampc-6zu1jja30gr.ws-eu89.gitpod.io/api/activities/home


✅ Run queries to explore traces within Honeycomb.io


Attach here traces from honeycomb screenshots.



![](assets/week2/Honeycomb%200.png)


![](assets/week2/Honeycomb1.png)


![](assets/week2/Honeycomb2.png)


![](assets/week2/Honeycomb3.png)






✅ Instrument AWS X-Ray into backend flask application

I followed Andrew [Guideline](https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-2/journal/week2.md) 


Instrument AWS X-Ray for Flask

```
export AWS_REGION="eu-north-1"
gp env AWS_REGION="eu-north-1"
```
Added xray sdk in requirements.txt

``` 
pip install -r requirements.txt

```


Installed python dependencies 
```
pip install -r requirements.txt
```

Updated app.py with following code.

```
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.ext.flask.middleware import XRayMiddleware

xray_url = os.getenv("AWS_XRAY_URL")
xray_recorder.configure(service='Cruddur', dynamic_naming=xray_url)
XRayMiddleware(app, xray_recorder)

```
Configured AWS X-Ray Resources.
Add ```aws/json/xray.json```

```
{
  "SamplingRule": {
      "RuleName": "Cruddur",
      "ResourceARN": "*",
      "Priority": 9000,
      "FixedRate": 0.1,
      "ReservoirSize": 5,
      "ServiceName": "Cruddur",
      "ServiceType": "*",
      "Host": "*",
      "HTTPMethod": "*",
      "URLPath": "*",
      "Version": 1
  }
}
```

Here its showing installed requirements and also xray.json added.

![](assets/week2/xray%20json%20added%20%2B%20installed%20requirements.png)


Cretaed group using aws command.

![](assets/week2/Group%20created%20on%20CLI.png)

AWS console showing ```Cruddur``` group is created.


![](assets/week2/Group%20is%20showing%20in%20AWS%20condole.png)

Added X-ray daemon to docker service in Docker compose file.

 ```
 xray-daemon:
    image: "amazon/aws-xray-daemon"
    environment:
      AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
      AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
      AWS_REGION: "us-east-1"
    command:
      - "xray -o -b xray-daemon:2000"
    ports:
      - 2000:2000/udp
  ```
      
      Added Environment variables also yo doceker-compose.yml
      
      ```
      AWS_XRAY_URL: "*4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}*"
      AWS_XRAY_DAEMON_ADDRESS: "xray-daemon:2000"
      ```

Samping Rule added in ```gitpod.yml``` file.

![](assets/week2/Sampling%20Rule%20CLI.png)


Sampling rule also showing in AWS CLI.

![](assets/week2/Sampling%20Rule%20at%20Console.png)


✅ Configure and provision X-Ray daemon within docker-compose and send data back to X-Ray API

After that step, I run the Docker compose UP.

![](assets/week2/Copmoser%20Up%20.png)

Then I opened the web link after clicking open ports and performed few operations like refresh the activities.

I clicked on 'Run Query' to fetch data.

![](assets/week2/Run%20Query%20in%20Traces.png)


Service map metrics.
![](assets/week2/Service%20map%20matrics.png)

Service Map
![](assets/week2/Service%20map.png)


![](assets/week2/Traces.png)


✅ Observe X-Ray traces within the AWS Console

I performed few more tests by refreshing the page etc.
![](assets/week2/More%20traces%20after%20doing%20some%20tests.png)

Service Map after more tests.

![](assets/week2/Service%20map%20after%20some%20tests.png)

After watching new SubSegment [video](https://www.youtube.com/watch?v=4SGTW0Db5y0) added code and its workingh fine and showing in AWS console.

![](assets/week2/Query%20Traces.png)


![](assets/week2/Sub%20Segments.png)


✅ Integrate Rollbar for Error Logging

## Rollbar

https://rollbar.com/

Create a new project in Rollbar called `Cruddur`

Added following to `requirements.txt`


```
blinker
rollbar
```

Installed dependencies

```sh
pip install -r requirements.txt
```

Copy the token code from rollbar web after login and set environment variables.

```sh
export ROLLBAR_ACCESS_TOKEN=""
gp env ROLLBAR_ACCESS_TOKEN=""
```

Added to backend-flask for `docker-compose.yml`

```yml
ROLLBAR_ACCESS_TOKEN: "${ROLLBAR_ACCESS_TOKEN}"
```

Imports done for Rollbar

```py
import rollbar
import rollbar.contrib.flask
from flask import got_request_exception
```

```py
rollbar_access_token = os.getenv('ROLLBAR_ACCESS_TOKEN')
@app.before_first_request
def init_rollbar():
    """init rollbar module"""
    rollbar.init(
        # access token
        rollbar_access_token,
        # environment name
        'production',
        # server root directory, makes tracebacks prettier
        root=os.path.dirname(os.path.realpath(__file__)),
        # flask already sets up logging
        allow_logging_basic_config=False)

    # send exceptions from `app` to rollbar, using flask's signal system.
    got_request_exception.connect(rollbar.contrib.flask.report_exception, app)
```

WAdded endpoint just for testing rollbar to `app.py`

```py
@app.route('/rollbar/test')
def rollbar_test():
    rollbar.report_message('Hello World!', 'warning')
    return "Hello World!"
```

[Rollbar Flask Example](https://github.com/rollbar/rollbar-flask-example/blob/master/hello.py)


Here my integration showing WARNING.

![](assets/week2/Rollbar%20Warning%201.png)


![](assets/week2/Rollbar%20warning%202.png)



✅ Trigger an error an observe an error with Rollbar

![](assets/week2/Rollbar%20Error%20on%20activities%20home.png)


![](assets/week2/Rollbar%20showing%20Error%201.png)


![](assets/week2/Rollbar%20showing%20Error%202.png)


 #  Homework Challenges 

✅ Instrument Honeycomb for the frontend-application to observe network latency between frontend and backend[HARD]



✅ Add custom instrumentation to Honeycomb to add more attributes eg. UserId, Add a custom span

For this task, I read [Guideline](https://docs.honeycomb.io/getting-data-in/opentelemetry/python/) and used openelementry in home_activities.py

 ```
from opentelemetry import trace

tracer = trace.get_tracer("home.activities")
 ```
 
 Spans created with.
 ```
 with tracer.start_as_current_span("home-activites-mock-data"):
      span = trace.get_current_span()
 ```

Tested backend app on port 4567 as howing in screenshot.

![](assets/week2/Backend%20showing%20data-.png)

Full Span graph

![](assets/week2/Total%20Spans.png)


Showing spans.

![](assets/week2/2%20spans%20showing.png)


![](assets/week2/2%20spans.png)


✅ Run custom queries in Honeycomb and save them later eg. Latency by UserID, Recent Traces

Run Query P90 and Heatmap  

![](assets/week2/P90%20and%20heatmap.png)


✅ Install WatchTower and write a custom logger to send application log data to - CloudWatch Log group

Added  the `requirements.txt`

```
watchtower
```

Installed the requirements.

```sh
pip install -r requirements.txt
```


In `app.py` added imports.

```
import watchtower
import logging
from time import strftime
```

Also added in app.py
```py
# Configuring Logger to Use CloudWatch
LOGGER = logging.getLogger(__name__)
LOGGER.setLevel(logging.DEBUG)
console_handler = logging.StreamHandler()
cw_handler = watchtower.CloudWatchLogHandler(log_group='cruddur')
LOGGER.addHandler(console_handler)
LOGGER.addHandler(cw_handler)
LOGGER.info("some message")
```

```py
@app.after_request
def after_request(response):
    timestamp = strftime('[%Y-%b-%d %H:%M]')
    LOGGER.error('%s %s %s %s %s %s', timestamp, request.remote_addr, request.method, request.scheme, request.full_path, response.status)
    return response
```

We'll log something in an API endpoint
```py
LOGGER.info('Hello Cloudwatch! from  /api/activities/home')
```

Set the env var in your backend-flask for `docker-compose.yml`

```yml
      AWS_DEFAULT_REGION: "${AWS_DEFAULT_REGION}"
      AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
      AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
```
After the request received the traces in AWS console cloud watch logs.

![](assets/week2/Cloud%20watcg%20logs%200.png)


Cloud Watch logs events showing.

![](assets/week2/Cloud%20watch%20logs%201.png)

Cloud watch cruddur group showing Logs and histogram praph after running Query.

![](assets/week2/Cloud%20watcgh%20logs%20after%20run%20query%20graph.png)






