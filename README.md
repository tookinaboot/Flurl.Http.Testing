# Flurl.Http.Testing

Flurl.Http.Testing is a variation of Flurl.Http,which modifies and extends features to provide support for things commonly needed to effectively test HTTP APIs from the API consumer's perspective.
This library also the unit testing features of Flurl.Http, which provides features needed to unit test code, which uses Flurl.Http to make HTTP calls.
Instead, this library's testing features are strictly related to consuming an API in a deployed environment - In other words, to integration test, not unit test the API.

## What is different or added from vanilla Flurl.Http

1. Modified behavior of HTTPClient to provide TestMethod isolation
2. Fluent skipping serialization of properties within HTTP bodies to enable easily testing things like backwards compatibility without having different versions of C# classes with JsonIgnore attributes.
3. Fluent asserts on IFluentResponse to make tests more readable

### Below is the original README.md content from fork: https://github.com/tmenier/Flurl

[![Build status](https://ci.appveyor.com/api/projects/status/hec8ioqg0j07ttg5/branch/master?svg=true)](https://ci.appveyor.com/project/kroniak/flurl/branch/master)
[![Flurl-stable](https://img.shields.io/nuget/v/Flurl.svg?maxAge=3600&label=Flurl%20nuget)](https://www.nuget.org/packages/Flurl/)
[![Flurl.Http-stable](https://img.shields.io/nuget/v/Flurl.Http.svg?maxAge=3600&label=Flurl.Http%20nuget)](https://www.nuget.org/packages/Flurl.Http/)
[![Flurl-pre-release](https://img.shields.io/nuget/vpre/Flurl.svg?maxAge=3600&label=Flurl%20Pre-Release%20nuget)](https://www.nuget.org/packages/Flurl/)
[![Flurl.Http-pre-release](https://img.shields.io/nuget/vpre/Flurl.Http.svg?maxAge=3600&label=Flurl.Http%20Pre-Release%20nuget)](https://www.nuget.org/packages/Flurl.Http/)

Flurl is a modern, fluent, asynchronous, testable, portable, buzzword-laden URL builder and HTTP client library.

````c#
var result = await "https://api.mysite.com"
    .AppendPathSegment("person")
    .SetQueryParams(new { api_key = "xyz" })
    .WithOAuthBearerToken("my_oauth_token")
    .PostJsonAsync(new { first_name = firstName, last_name = lastName })
    .ReceiveJson<T>();

[Test]
public void Can_Create_Person() {
    // fake & record all http calls in the test subject
    using (var httpTest = new HttpTest()) {
        // arrange
        httpTest.RespondWith("OK", 200);

        // act
        await sut.CreatePersonAsync("Claire", "Underwood");
        
        // assert
        httpTest.ShouldHaveCalled("http://api.mysite.com/*")
            .WithVerb(HttpMethod.Post)
            .WithContentType("application/json");
    }
}
````

Get it on NuGet:

`PM> Install-Package Flurl.Http`

Or get just the stand-alone URL builder without the HTTP features:

`PM> Install-Package Flurl`

For updates and announcements, [follow @FlurlHttp on Twitter](https://twitter.com/intent/user?screen_name=FlurlHttp).

For detailed documentation, please visit the [main site](https://flurl.dev). 
