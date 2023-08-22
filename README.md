# Sendcloud-servicepoints-patch
Fixes an issue with the sendcloud service points in which quotes and orders would not have sendcloud data

We tested this in the checkout (which runs the onestepcheckout_iOSC). The tricky part was finding the conditions under which the quote table did not fill. So to give you an image of this checkout: the address fields are in the top left of the page and the delivery method selection in the top right. As far as a customer is concerned, he can fill either one first.

Condition under which the problems occur (for us at least): A customer fills in part of the address (like name, address etc), but still leaves some fields open that are required (like the phone number in this instance). It then first selects the delivery point before, lets say, entering the required phone number. The delivery point on the frontend gets selected, but it wont be able to load the part of the script that sends the sendcloud pickup point data to the quote. We can actually see it not-happing, both trough the database, but also because the delivery method spinner won't activate.
The customer then proceeds to fill in the rest of his address fields and select the shipping method and proceeds to place the order. In all these parts the quote won't be filled anymore with the sencloud data.
The verification lets it through because all fields are filled, but the sencloud data in the quote is still empty. This quote data is to be set in the new order. So empty servicepoints in quote, empty servicepoints in order, empty servicepoints in the sencloud panel.

For now we fixed it with a composer patch. Validate.js seems to run before placeOrder. We added the function that sends the shipping information to the quote again so it wont be empty. 
