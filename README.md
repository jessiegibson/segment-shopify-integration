Segment integration into Shopify. 

Each of these scripts will be placed on each of these theme films. ** Depending on the theme, the Query Selectors will need to be updated for each of the forms, buttons, etc.

# Shopify Pages updated


# THEME.LIQUID
!function(){var analytics=window.analytics=window.analytics||[];if(!analytics.initialize)if(analytics.invoked)window.console&&console.error&&console.error("Segment snippet included twice.");else{analytics.invoked=!0;analytics.methods=["trackSubmit","trackClick","trackLink","trackForm","pageview","identify","reset","group","track","ready","alias","debug","page","once","off","on","addSourceMiddleware","addIntegrationMiddleware","setAnonymousId","addDestinationMiddleware"];analytics.factory=function(e){return function(){var t=Array.prototype.slice.call(arguments);t.unshift(e);analytics.push(t);return analytics}};for(var e=0;e<analytics.methods.length;e++){var key=analytics.methods[e];analytics[key]=analytics.factory(key)}analytics.load=function(key,e){var t=document.createElement("script");t.type="text/javascript";t.async=!0;t.src="https://cdn.segment.com/analytics.js/v1/" + key + "/analytics.min.js";var n=document.getElementsByTagName("script")[0];n.parentNode.insertBefore(t,n);analytics._loadOptions=e};analytics.SNIPPET_VERSION="4.13.1";

analytics.load("WRITE KEY");
analytics.page();


# CHECKOUT - ADDITIONAL SCRIPTS SECTION

!function(){var analytics=window.analytics=window.analytics||[];if(!analytics.initialize)if(analytics.invoked)window.console&&console.error&&console.error("Segment snippet included twice.");else{analytics.invoked=!0;analytics.methods=["trackSubmit","trackClick","trackLink","trackForm","pageview","identify","reset","group","track","ready","alias","debug","page","once","off","on","addSourceMiddleware","addIntegrationMiddleware","setAnonymousId","addDestinationMiddleware"];analytics.factory=function(e){return function(){var t=Array.prototype.slice.call(arguments);t.unshift(e);analytics.push(t);return analytics}};for(var e=0;e<analytics.methods.length;e++){var key=analytics.methods[e];analytics[key]=analytics.factory(key)}analytics.load=function(key,e){var t=document.createElement("script");t.type="text/javascript";t.async=!0;t.src="https://cdn.segment.com/analytics.js/v1/" + key + "/analytics.min.js";var n=document.getElementsByTagName("script")[0];n.parentNode.insertBefore(t,n);analytics._loadOptions=e};analytics.SNIPPET_VERSION="4.13.1";
	window.analytics.load("WRITE KEY");

analytics.identify({{ customer.id }}, {
	userId: {{ customer.id }},
	email: '{{ customer.email }}',
	first_name:"{{ customer.first_name }}",
	last_name:"{{ customer.last_name }}",
	street: "{{ customer.default_address.street }}",
	city: "{{ customer.default_address.city }}",
	state: "{{ customer.default_address.province }}",
	postalCode: "{{ customer.default_address.zip }}",
	country: "{{ customer.default_address.country }}",
	order_id: "{{ order.id }}",
	allOrdersCount: {{ customer.orders_count }},
	last_updated: new Date()
	});
	{% if first_time_accessed %}

analytics.track('Completed Order',{
	userId: {{ customer.id }},
	first_name:"{{ customer.first_name }}",
	last_name:"{{ customer.last_name }}",
	order_id: {{ order.id }},
	products: [
		{% for line_item in line_items %}
		    {
		      id: "{{ line_item.id }}",
		      name: "{{ line_item.title }}",
		      price: "{{ line_item.price }}",
		      quantity: {{ line_item.quantity }}
		    },
		    {% endfor %} ],
	gross_sales: {{total_price | money_without_currency }},
	net_sales: {{total_price | money_without_currency }},
	shipping: {{total_price | money_without_currency }},
	tax: {{total_price | money_without_currency }}
	});
	{% else %}

analytics.track('Viewed Order',{
	userId: {{ customer.id }},
	first_name:"{{ customer.first_name }}",
	last_name:"{{ customer.last_name }}",
	order_id: {{ order.id }},
	products: [
		{% for line_item in line_items %}
		    {
		      id: "{{ line_item.id }}",
		      name: "{{ line_item.title }}",
		      price: "{{ line_item.price }}",
		      quantity: {{ line_item.quantity }}
		    },
		    {% endfor %} ],
	gross_sales: {{total_price | money_without_currency }},
	net_sales: {{total_price | money_without_currency }},
	shipping: {{total_price | money_without_currency }},
	tax: {{total_price | money_without_currency }}
	{% endif %}
}}();

	
	

# ARTICLE-TEMPLATE.LIQUID
analytics.track('Viewed Blog Post',{
    blog_title: "{{ article.title }}",
    blog_author: "{{ article.author }}",
    blog_created_dt: "{{ article.created_at | date: "%F" }}",
	  blog_published_dt: "{{ article.published_at | date: "%F" }}",
    blog_comment_count: {{article.comments_count }},
    blog_tags: ["{{ article.tags | join: '", "'}}"],
    blog_url: "{{ article.url }}",
    referrer_url: document.referrer,
    referrer_path: new URL(document.referrer).pathname
    });


# CART.LIQUID
var the_cart = document.querySelector('#the-cart');
analytics.trackForm(the_cart,"Initiate Checkout", {
	    item_count: {{ cart.item_count }},
	    cart_subtotal: {{ cart.items_subtotal_price | money_no_currency }},
	    cart_currency: "{{ cart.currency.iso_code }}",
	    total_price: {{ cart.total_price | money_no_currency}},
	    cart_items: [
	        {% for item in cart.items %}
	        {
	        id: "{{ item.id }}",
	        quantity: "{{ item.quantity }}",
	        product_name: "{{ item.title }}",
	        price: {{ item.line_price | money_no_currency }},
	        },
	    {% endfor %}],
	    });


# CUSTOMERS/ACCOUNT.LIQUID

analytics.identify({{ customer.id }}, {
	userId: {{ customer.id }},
	email: '{{ customer.email }}',
	first_name:"{{ customer.first_name }}",
	last_name:"{{ customer.last_name }}",
	street: "{{ customer.default_address.street }}",
	city: "{{ customer.default_address.city }}",
	state: "{{ customer.default_address.province }}",
	postal_code: "{{ customer.default_address.zip }}",
	country: "{{ customer.default_address.country }}",
	total_spent: "{{ customer.total_spent }}",
	all_orders_count: {{ customer.orders_count }},
	last_updated: new Date()
});
  
analytics.track('Viewed Account Screen', {
	userId: {{ customer.id }},
      	email: '{{ customer.email }}',
      	first_name: '{{ customer.first_name }}',
      	last_name: '{{ customer.last_name }}'
   	});
    
document.querySelector('.section-header__link').onclick = function user_logout(){
	analytics.track('Logged Out');
      	analytics.reset();
};


# CUSTOMER/LOGIN.LIQUID
analytics.track('Viewed Login Page');
          
var cust_login = document.querySelector('#CustomerLoginForm');
var login_email = document.querySelector('#CustomerEmail');

analytics.track('Logged In - Account',{
	email: login_email.value
	});

{% if {{customer.id  %}
analytics.identify({{ customer.id}},{
	userId: {{ customer.id }},
        email: {{ customer.email }},
        login_dt: new Date()
});
{% endif %}


# CUSTOMER/REGISTER.LIQUID
var create_cust = document.querySelector('create_customer')  
var fname = document.querySelector('#FirstName').text;
var lname = document.querySelector('#LastName').text;
var email = document.querySelector('#Email').text;          
              
analytics.trackSubmit(create_cust,'Registered Account',{
	first_name: fname,
	last_name: lname,
	email: email
	});
       
{% if {{customer.id}} %}
analytics.identify({{ customer.id }},{
	userId: {{customer.id}},
	first_name: "{{ customer.first_name }}",
	last_name: "{{ customer.last_name }}",
	email: "{{ customer.email }}"
});
{% endif %}


# COLLECTION-TEMPLATE.LIQUID
  	
analytics.track('Viewed Collection Page', {
	collection: "{{ collection.title }}",
	collection_url: "{{ collection.url }}",
	collection_count: {{ collection.all_products_count }}
	});

# PRODUCT-FORM.LIQUID

var add_to_cart = document.querySelector('.product-single__form');

analytics.trackForm(add_to_cart,'Added Product to Cart',{
		id: "{{ product.id }}",
		name: "{{ product.title }}",
		type: "{{ product.type }}",
		url: "{{ product.url }}",
		product_type: "{{ product.type }}",
		image_url: "[{{ product | img_url }}]",
		variant_selected: "{{ product.selected_variant }}",
		price: {{ product.price | money_without_currency }},
		price_min: {{ product.price_min | money_without_currency  }},
		price_max: {{ product.price_max | money_without_currency  }},
		sale: {{ product.price_varies }},
		variants_count: "{{ product.variants.size }}",
		available: "{{ product.available }}",
		options_count: {{ product.options.size }},
	    });


# SEARCH.LIQUID

analytics.track('Searched Products', {
	results_count: {{ search.results_count }},
	search_terms: "{{ search.terms }}"
        });

