# prawn-lightning-talk

## Install Ruby Gem
1. Add to the Rails Gemfile
```
gem 'prawn-rails'
```

## Create PDF Response

2. In your controller, you can now request different formats on a single view
```
def show
     @order = Order.find(params[:id])
 	respond_to do |format|
 	  format.html
 	  format.pdf do
 	    pdf = Prawn::Document.new
 	    pdf.text "Hello world"
 	    send_data pdf.render, 
 	end
 end
```

## Add Link to PDF
To access the html, follow your usual route: /orders/:id
To access the pdf, add '.pdf' to the end of the original route
```
<%= link_to 'Print Receipt (PDF)', order_path(@order, format: "pdf") %>
```

## Formatting

```
send_data pdf.render,
     filename: "order ##{@order.id}.pdf",
     type: "application/pdf",
     disposition: "inline"
```


## Initialize

In a new OrderPdf Model, you can create a template when a OrderPdf.new is created
```
class OrderPdf < Prawn::Document
	def initialize(order)
		super(options = {page_layout: :landscape, top_margin: 100})
		@order = order
		text "Thank you for shopping at Fruits & Stationery"
		message_to_owner
		list_items
	end

	def message_to_owner
		text "Order ##{@order.id} belongs to #{@order.owner}."
	end

	def list_items
		@order.items.each do |item|
			text "$#{item.price} - #{item.name}"
		end
	end
end
```
### References:
* http://prawnpdf.org/docs/0.11.1/Prawn/Document.html
* http://prawnpdf.org/manual.pdf
* https://www.youtube.com/watch?v=vp3nrafhjEc
