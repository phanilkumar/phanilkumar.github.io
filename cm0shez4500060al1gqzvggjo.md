---
title: "Integrating JSONAPI::Deserialization"
seoTitle: "JSONAPI::Deserialization Integration Guide"
seoDescription: "Integrate JSONAPI::Deserialization into your Rails ApplicationController with step-by-step instructions for handling JSONAPI requests"
datePublished: Sat Sep 07 2024 18:31:55 GMT+0000 (Coordinated Universal Time)
cuid: cm0shez4500060al1gqzvggjo
slug: integrating-jsonapideserialization
canonical: https://claude.site/artifacts/f494128e-152a-4059-8764-c380424953ff
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/gI7zgb80QWY/upload/00458e3f4d675aae0a6dfe4a2c879e2f.jpeg
tags: ruby, programming, ruby-on-rails

---

```ruby
require 'jsonapi/deserializable'

class ApplicationController < ActionController::API
  include JSONAPI::Deserialization

  before_action :deserialize_jsonapi_request

  private

  def deserialize_jsonapi_request
    return unless request.content_type == 'application/vnd.api+json'

    hash = request.request_parameters
    hash = hash.to_unsafe_hash if Rails::VERSION::MAJOR >= 5
    ActiveModelSerializers::Deserialization.jsonapi_parse!(hash)
  rescue ActiveModelSerializers::Deserialization::InvalidDocument => e
    render jsonapi_errors: e.errors, status: :unprocessable_entity
  end
end
```

1. We require the jsonapi/deserializable library to access the JSONAPI::Deserialization module.
    
2. Inside the `ApplicationController`, we include the `JSONAPI::Deserialization` module. This mixes in the deserialization functionality into the controller.
    
3. We define a `before_action` callback that calls the `deserialize_jsonapi_request` method before each action in the controller.
    
4. In the `deserialize_jsonapi_request` method:
    
    4.1 We check if the request's `content_type` is `application/vnd.api+json`. If
    
    not, we skip deserialization.
    
    4.2 We retrieve the request parameters using `request.request_parameters`.
    
    4.3 If using Rails 5 or later, we convert the parameters to an unsafe hash
    
    using `to_unsafe_hash` for compatibility.
    
    4.4 We call `ActiveModelSerializers::Deserialization.jsonapi_parse!` to parse
    
    and deserialize the JSONAPI request parameters.  
    4.5 If an `ActiveModelSerializers::Deserialization::InvalidDocument` exception
    
    is raised during deserialization (indicating invalid JSONAPI input), we
    
    render a JSONAPI error response with a status of`:unprocessable_entity`.
    
5. With this setup, the `JSONAPI::Deserialization` module will be included in the `ApplicationController`, and the `deserialize_jsonapi_request` method will be called before each action to deserialize the incoming JSONAPI request parameters.
    
6. Remember to have the `jsonapi-serializer` gem (or a similar JSONAPI serialization/deserialization library) installed in your application for this code to work.