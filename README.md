# HyperactiveResource

HyperActiveResource extends ActiveResource so it works properly and behaves more like ActiveRecord

Many have said that ActiveResource is not really "complete". On the surface, this means that some features that are documented aren't implemented. Digging a little deeper, we find that some features that should exist don't.

Arguably, a "complete" ActiveResource would behave like ActiveRecord or, as the rdoc for ActiveResource states "very similarly to Active Record".

Hyperactive Resource is MDL's extension to ActiveResource::Base written to support our Patient Registry and goes a long way towards the goal of an ActiveResource that behaves like ActiveRecord.

## Features
 * Client side validations
 * Hooks for before_validate, before_save
 * Dynamic finders: find_by_X
 * save!
 * Awareness of associations between resources: belongs_to, has_many, has_one & columns
   * Patient.new.name returns nil instead of MethodMissing
   * Patient.new.races returns [] instead of MethodMissing
   * pat = Patient.new; pat.gender_id = 1; pat.gender #Will return find the gender obj
 * Resources can be associated with records
 * Records can be associated with records
 * ActiveRecord-like attributes= (updates rather than replaces)
 * ActiveRecord-like #load that doesn't #dup attributes (stores direct reference)
 * Supports saving resources that :include other resources via:
   * Nested resource saving (creating a patient will create their associated addresses)
   * Mapping associations ([:gender].id will serialize as :gender_id)

### Example

 1. Create a HyperactiveResource where you would normally use ActiveResource
    and define the meta-data/associations that drive the dynamic magic:

  ```ruby
  class Address < HyperactiveResource
    self.columns = [ :street_address, :city, :zipcode, :home_phone_number ]
    self.belong_tos = [ :country, :state ]
    self.has_manys = [ :people ]
  end
  ```

 3. Enjoy the magic

  ```ruby
  address = Address.new
  address.country # nil instead of method_missing
  address.country_id = 5
  address.country #Returns Country.find(5)
  ```

  etc..

Copyright (c) 2008 Medical Decision Logic, 2013 Massimo Maino released under the MIT license
