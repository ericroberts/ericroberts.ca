---
layout: post
title:  "Building Objects in the Session with Rails and Wicked Wizard"
date:   2013-08-07
categories: ruby
---

Recently I stumbled across a great gem, [Wicked Wizard](https://github.com/schneems/wicked). As I dug in further I noticed they had a great guide for [building partial objects step by step](https://github.com/schneems/wicked/wiki/Building-Partial-Objects-Step-by-Step). However, it didn't seem to do quite what I wanted. I was looking for something that:

1. <span>Doesn't require the model have knowledge of the wizard in order to perform validations.</span>
2. <span>Doesn't require me to clean up incomplete objects when someone didn't go through the whole wizard.</span>

I decided to start tackling requirement 2 first, since I had an idea how to go about that. I figured if I stored objects in the session, I could just keep building it there until it came time for the final submission. I googled for a solution and the closest thing I could find was [this gist](https://gist.github.com/kizzx2/4722784) about using the session to store a partial object. When I tried to use it though, it didn't work for me. Based on the gist though, I was able to come up with a solution which I'll detail below. I'll use the same example as the Wicked Wizard guide (a product).

First, let's create our product class:
{% highlight ruby linenos %}
class Product < ActiveRecord::Base
  validates :name, :price, :category, :presence => true
end
{% endhighlight %}

Next, let's begin creating our controller. Starting from what Wicked Wizard tells us to do, we'd have something like this:
{% highlight ruby linenos %}
class ProductWizardController < ApplicationController
  include Wicked::Wizard

  steps :add_name, :add_price, :add_category

  def show
    @product = Product.find(params[:product_id])
    render_wizard
  end

  def update
    @product = Product.find(params[:product_id])
    @product.update_attributes(params[:product])
    render_wizard @product
  end

  def create
    @product = Product.create
    redirect_to wizard_path(steps.first, :product_id => @product.id)
  end
end
{% endhighlight %}

But the `Product.find(params[:product_id])` and `Product.create` lines aren't going to work for us anymore. In fact, it doesn't make sense to have a create action at all, since we don't create a product to start. So let's remove the create action and put a new action in it's place:

{% highlight ruby linenos %}
def new
  product = WizardProduct.new

  session[:product_wizard] ||= {}
  session[:product_wizard][:product] = product.accessible_attributes

  redirect_to wizard_path(steps.first)
end
{% endhighlight %}

You haven't seen the WizardProduct class yet, but we'll get to that in a minute. What we've done is instantiated a new product, stored any data that product has in the session, and then redirected to step one of our wizard, `:add_name`. As I mentioned earlier, we can't use `Product.find` anymore, so let's change our show action to the following:

{% highlight ruby linenos %}
def show
  @product = WizardProduct.new(session[:product_wizard][:product])
  @step = step

  render_wizard
end
{% endhighlight %}

Instead of going and finding our object like we would with a database, we're going to instantiate a new one each time with the attributes we've stored in the session. Initially I tried storing the whole object in the session but due to limitations on how much you can store there this would consistently fail.

Next, we'll also need to remove `Product.find` in the update action:
{% highlight ruby linenos %}
def update
  @product = WizardProduct.new(session[:product_wizard][:product])
  @product.attributes = params[:product]

  @product.step = step
  @product.steps = steps
  @product.session = session
  @product.validations = validations

  render_wizard @product
end
{% endhighlight %}

Now things are getting a bit more complicated and it's time to discuss the WizardProduct class. What we've done so far is to instantiate our WizardProduct object with the attributes stored in the session, and we've added a bunch of data to it that our WizardProduct class will need to do it's job. Let's take a look at WizardProduct:

{% highlight ruby linenos %}
class WizardProduct < Product
  attr_accessor :step, :steps, :session, :validations
  
  @@parent = superclass
  
  def underscored_name
    @@parent.name.underscore
  end
  
  def self.model_name
    @@parent.model_name
  end
  
  def save
    valid?
    
    remove_errors_from_other_steps
    
    if errors.empty?
      if step == steps.last
        obj = @@parent.new(accessible_attributes)
        
        session.delete("#{underscored_name}_builder".to_sym) if obj.save
      else
        session["#{underscored_name}_builder".to_sym][underscored_name.to_sym] = accessible_attributes
      end
    end
  end
  
  def accessible_attributes
    aa = @@parent.accessible_attributes
    attributes.reject! { |key| !aa.member?(key) }
  end
  
  def remove_errors_from_other_steps
    other_step_validation_keys = (errors.messages.keys - validations[step])
    errors.messages.reject! { |key| other_step_validation_keys.include?(key) }
  end
end
{% endhighlight %}
