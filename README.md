## Shepherd

**Warning: Still in development. Not ready for production use**

[![Build Status](https://travis-ci.org/bustlelabs/shepherd.svg?branch=master)](https://travis-ci.org/bustlelabs/shepherd)

A framework for building resources using AWS resource Gateway and Lambda

## Installation

`npm install -g shepherd-cli`

## Quick start

```
shepherd create project
... follow prompts ...
cd <projet-folder>
git init
cp env.js.example env.js
npm install
shepherd create function
... follow prompts ...
shepherd create resource
... follow prompts ...
shepherd create method
... follow prompts ...
shepherd deploy
... follow prompts ...
```

## Opinions

## Mapping templates

API Gateway allows you to write "mapping templates" that transform data before sending it to lambda or other backends. They are written in velocity (java templating) and generally not fun to deal with. Shepherd contains a generic template that you should use instead. This is automatically configured for you. Functions generated by shepherd contain ES2015 destructuring that already matches this template. Just use it and save yourself some headache.

Here is an example of the input to lambda:

``` js
{
  headers: {
    Accept: "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8",
    Accept-Encoding: "gzip",
    Accept-Language: "en-US,en;q=0.8",
    Cache-Control: "max-age=0",
    CloudFront-Forwarded-Proto: "https",
    CloudFront-Is-Desktop-Viewer: "true",
    CloudFront-Is-Mobile-Viewer: "false",
    CloudFront-Is-SmartTV-Viewer: "false",
    CloudFront-Is-Tablet-Viewer: "false",
    CloudFront-Viewer-Country: "US",
    DNT: "1",
    Upgrade-Insecure-Requests: "1",
    Via: "1.1 9fab2a39c6d2bda91a3a58e2e1e6133d.cloudfront.net (CloudFront)",
    X-Amz-Cf-Id: "_iTvfyle3-4VqQD3doABOKcjRMq06s1CGQXGTr6LBZB4Wm0i5T2ebg==",
    X-Forwarded-For: "75.151.133.222, 204.246.168.164",
    X-Forwarded-Port: "443",
    X-Forwarded-Proto: "https"
  },
  pathParameters: { },
  queryParameters: { },
  body: { }
}
```

### Lambda versions + API Gateway stages

Shepherd automatically configures proper permissions and links between API Gateway stages and lambda functions. Example: the beta API stage will call the beta version of your lambda functions. Environment variables are contained in `env.js` and copied into your lambda function on deployment. Just add new keys to that file to created additional environments.

### Node Dependencies

Dependencies can be specified for all project functions in your project root `package.json`. These are copied to each function on deployment and overidden by dependencies in each functions `package.json` file.

## Commands

`shepherd create project`

Creates a new project

`shepherd create resource`

Creates a new resource

`shepherd create function`

Creates a new function

`shepherd create method`

Creates a new method. You should already have created the resource and the function before running this command

`shepherd deploy`

Deploys all functions, sets up permissions+versions, and makes a new API gateway deployment 

`shepherd pull`

Pulls a JSON representation of your API and writes it to `api.json`. This is used by shepherd to match up functions with resources and endpoints. If you make changes using the API gateway web UI make sure to pull down those changes by running `shepherd pull`

## Development

Rebuild on file change: `npm run compile -- -w`
