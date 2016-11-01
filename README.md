# prawn-lightning-talk

## Install Ruby Gem
1. Add to the Rails Gemfile
```
gem 'prawn-rails'
```
## Enable PDF Response
2. Create a Mime Type in config/initializers/mime_types
> Mime::Type.register "application/pdf", :pdf

* A Mime type (or media type) is a two-part identifier for file formats and format contents
* This allows our Rails App to know how to respond to a pdf request
	* Common examples: application/json, application/pdf, text/html, image/png, image/jpg

## Create PDF Response

3. In your controller, you can now request different formats on a single view

> def show
>     @order = Order.find(params[:id])
> 	respond_to do |format|
> 	  format.html
> 	  format.pdf do
> 	    pdf = Prawn::Document.new
> 	    pdf.text "Hello world"
> 	    send_data pdf.render, 
> 	end
> end

## Add Link to PDF
To access the html, follow your usual route: /orders/:id
To access the pdf, add '.pdf' to the end of the original route

> <%= link_to 'Print Receipt (PDF)', order_path(@order, format: "pdf") %>


### References:
* http://prawnpdf.org/docs/0.11.1/Prawn/Document.html
* http://prawnpdf.org/manual.pdf
* https://www.youtube.com/watch?v=vp3nrafhjEc
