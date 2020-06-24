This reproduces a new bug in google cloud functions when deploying a pubsub message with vendored dependencies: 

` gcloud functions deploy HelloPubSub --runtime go113 --trigger-topic=mood-sdm
Deploying function (may take a while - up to 2 minutes)...failed.`


will result in: 

`ERROR: (gcloud.functions.deploy) OperationError: code=3, message=Build failed: # serverless_function_app/main
src/serverless_function_app/main/main.go:24:38: cannot use p.HelloPubSub (type func(context.Context, p.PubSubMessage) error) as type func(http.ResponseWriter, *http.Request) in argument to funcframework.RegisterHTTPFunction; Error ID: 6191efcd`

Note that if you remove .gcloudignore and the vendor directory, you can deploy successfully. 


Tickets I have found: 

https://github.com/GoogleCloudPlatform/functions-framework-go/issues/30

https://issuetracker.google.com/issues/159608139

https://github.com/terraform-providers/terraform-provider-google/issues/6662