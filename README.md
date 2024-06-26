# ottu-android-sdk

The OttuCheckout is iOS SDK makes it quick and easy to build an excellent payment experience in your iOS app. We provide powerful and customizable UI screens and elements that can be used out-of-the-box to collect your user's payment details. We also expose the low-level APIs that power those UIs so that you can build fully custom experiences.

## Features

**Simplified security**: We make it simple for you to collect sensitive data such as credit card numbers and remain PCI compliant. This means the sensitive data is sent directly to Stripe instead of passing through your server.

**Native UI**: We provide native screens and elements to collect payment details.

<p float="left">
<img src="https://github.com/sdkpayment/OttuCheckoutAndroidV1/blob/ayush_v1/PayoutScreen.jpg" alt="PaymentUI" align="center"  width="200" height="400"/>
<!-- <img src="https://github.com/Maninder1991/screens/blob/main/WithCardPayment.png" alt="PaymentUI" align="center"  width="200" height="400"/> -->

**Localized**: We support the following localizations: English, Arabic.

#### Privacy

The OttuCheckout SDK collects data to help us improve our products and prevent fraud. This data is never used for advertising and is not rented, sold, or given to advertisers.

## Requirements

The OttuCheckout requires IDE to develop android app. Sdk is compatible with minimum sdk 22 and above.

## Getting started

To initialize the SDK you need to create session token. 
You can create session token with our public API [Click here](https://app.apiary.io/iossdk2/editor) to see more detail about our public API.
    
Installation
==========================

#### Installation with dependecy

Put below dependency into your gradle

```java
allprojects {
  repositories {
	...
	maven { url 'https://jitpack.io' }
  }
}
    
dependencies {
       implementation 'com.github.ottuco:ottu-android-private-sdk:1.0.30'
}
```

Below is the sample code of how you can use Ottu Payment SDK.

```java
	
  Ottu ottuPaymentSdk = new Ottu(MainActivity.this);
                        ottuPaymentSdk.setApiId("apiKey"); // set Api Key which is get from Ottu merchant account
                        ottuPaymentSdk.setMerchantId("merchant_id");
                        ottuPaymentSdk.setSessionId("session_id"); // Retrive from public API
                        ottuPaymentSdk.setAmount("100.00"); // String Value
                        ottuPaymentSdk.setLocal("lang"); // en or ar
                        ottuPaymentSdk.build();
	
```

Get payment result in onActivityResult menthod in Activity.
	
```java
	  @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK ){
            if (requestCode == OttuPaymentResult ){
                SocketRespo paymentResult = (SocketRespo) data.getSerializableExtra("paymentResult");
		String Status = paymentResult.getStatus();
                if (Status.equals("success")){
                    	// handle success result
                	textView.setText(paymentResult.status);   
	        	textView.setText(paymentResult.message);
	        	textView.setText(paymentResult.order_no);
	        	textView.setText(paymentResult.operation);
                }else if (Status.equals("failed")){
                    	// handle failed result
                } else if (Status.equals("cancel")){
			// handle cancel result
		}
            }

        }
    }
	
```
if you are using fragments you will need to add broadcastReseiver in OnActivityResult to add your success-failure logic in your fragment.

```java
PAYMENT_SUCCESS = "paymentSuccess" // add in your constant accordingly
 @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK ){
            if (requestCode == OttuPaymentResult ){
                SocketRespo paymentResult = (SocketRespo) data.getSerializableExtra("paymentResult");
                textView.setText(paymentResult.status);   // success || failed || cancel
		String Status = paymentResult.getStatus();
                if (Status.equals("success")){
                    	// handle success result
 			Intent  intent = Intent(PAYMENT_SUCCESS)
               		sendBroadcast(intent)
                }else if (Status.equals("failed")){
                    	// handle failed result
                } else if (Status.equals("cancel")){
                   	// handle cancle result
                }
            }
        }
    }

// register your broadcast
activity.registerReceiver(
                paymentReceiver,
                IntentFilter(PAYMENT_SUCCESS)
)
	
// then you will receive it in your fragment with your action PAYMENT_SUCCESS
BroadcastReceiver paymentReceiver = new BroadcastReceiver() {
           @Override
            public void onReceive(Context context, Intent intent) {
               if(intent.getAction().equals(PAYMENT_SUCCESS)){
                  // add your code 
              }
            }
};
```

 ## STC Pay integration

 Set Stc Pay button in xml
 ```java
  <Ottu.util.StcPayButton
   android:id="@+id/stcBtn"
   android:layout_gravity="right"
   android:layout_width="100dp"
   android:layout_height="40dp"/>

```
Set creadential and transaction detail for make payment before user click on stc pay button
```java
  stcPayButton.setApiId("API_key");
        stcPayButton.setMerchantId("Mercent_id");
        stcPayButton.setLocal("en");  // Language
        stcPayButton.setCreateTransaction("e_commerce"
                , String.valueOf(amount) // Amount
                ,"KWD"                   // Currency code
                ,"https://postapp.knpay.net/disclose_ok/" //  Discloser url
                ,"https://postapp.knpay.net/redirected/"  //  Redirect url
                ,"customer_id"
                ,"1234567890"   // Mobile number
                ,"2:00");       // Transaction Time Limite

//   Get payment result in onActivityResult menthod in Activity
        @Override
        protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK ){
            if (requestCode == OttuPaymentResult ){
                SocketRespo paymentResult = (SocketRespo) data.getSerializableExtra("paymentResult");
                textView.setText(paymentResult.status);   // success || failed || cancel
	        textView.setText(paymentResult.message);
	        textView.setText(paymentResult.order_no);
	        textView.setText(paymentResult.operation);
            }

        }
    }


```

## ProGuard

 You may need to include the following lines in your progard-rules.pro file if enable progard or minifyEnble.
```java
-keep class Ottu** { *; }
```
	
## Licenses

- [OttuCheckout License](https://github.com/ottuco/ottu-android-sdk/blob/main/LICENCE)
