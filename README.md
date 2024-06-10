go-tinyfaas
----------------
[tinyFaaS](https://github.com/OpenFogStack/tinyFaaS) wrapper to manage and deploy functions to a remote tinyFaaS server.

## Usage
```go
const functionsPath = "../faas/tinyfaas/" // a local path of functions, e.g. tinyfaas directory
// initialize tinyFaaS manager instance
tf := TinyFaaS.New("localhost", "8080", functionsPath)  // tinyFaaS's management port address is 8080 by default

// upload a function from local, assuming the function is in the functionsPath by default
respU, err := tf.UploadLocal("sieve", "test/fns/sieve-of-eratosthenes", "nodejs", 1)
// upload a function from local with full path, and environment variables
respU, err := tf.UploadLocal("sieve", "full-path/to/sieve-of-eratosthenes", "nodejs", 1, true, args)
// upload a function from a remote URL
respU, err := tf.UploadURL("sieve", "tinyFaaS-main/test/fns/sieve-of-eratosthenes", "nodejs", 1, "https://github.com/OpenFogStack/tinyFaas/archive/main.zip")

// Check tinyFaaS response. (inc. timeout, ok, other errors?)
if err != nil {
log.Fatalln("error when calling tinyFaaS: ", err)
}
// Print the response
log.Infof("upload success:\n%s", respU) // 200 success, 400 bad request

// delete a function
errD := tf.Delete("sieve")

// Check tinyFaaS response. (inc. timeout, ok, other errors?)
if errD != nil {
log.Fatalln("error when calling tinyFaaS: ", errD)
}

// get results log
respL, errL := tf.ResultsLog()

// Check tinyFaaS response. (inc. timeout, ok, other errors?)
if errL != nil {
log.Fatalln("error when calling tinyFaaS: ", errL)
}
// Print the response
fmt.Printf("results log: %s\n", respL) // 200 success

// list functions
respF := tf.Functions()
// Print the response
fmt.Printf("functions: %s\n", respF)

// remove all functions
tf.WipeFunctions()

// call a function
tf.Call("sieve", "10")

```