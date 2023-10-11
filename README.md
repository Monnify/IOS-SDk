# IOS-SDk

### An Integration Guide for the Monnify IOS SDK


With the Monnify IOS SDK, you are able to receive payments in your IOS application via:

*  Card
* Bank Transfer
* USSD
* Phone Number

Outline below are the necessary steps required to integrate the IOS SDK

1. ##### Add the iOS SDK to your project via CocoaPods
  Monnify iOS SDK is available through [CocoaPods](https://cocoapods.org). To install it, simply add the following line to your Podfile:
  ```
  pod 'MonnifyiOSSDK'
  ```

   
  

2. ##### Instantiate the Monnify SDK
   Next you are to create an instance of the Monnify sdk

   ```
   let monnify = Monnify.shared
   ``` 
     
3. ##### Configure your merchant API key and contract code
   This should be done in your `AppDelegate.swift` file and should only be done once in the entire application.  

   The environment can be TEST or LIVE. The TEST mode works on a sandbox environment and account transfer payments can be simulated [here](https://websim.sdk.monnify.com/#/bankingapp).

   Remember to switch to ApplicationMode.live when generating live builds for your application on production.

   ```
   let apiKey = "MK_PROD_XXXXXXXX"
   let contractCode = "1234567890"
   let mode = ApplicationMode.test

   monnify.setApplicationMode(applicationMode: mode)
   monnify.setApiKey(apiKey: apiKey)
   monnify.setContractCode(contractCode: contractCode)
   ```

4. ##### Set transaction request payload
    Set the necessary request parameters as shown below:
   
```
   let amount = Decimal(100)
   let paymentRef = "ASDF123454321"

   let parameter = TransactionParameters(amount: amount,
                                         currencyCode: "NGN",
                                         paymentReference: paymentRef,
                                         customerEmail: "johndoe@example.com",
                                         customerName: "John Doe" ,
                                         customerMobileNumber: "08000000000",
                                         paymentDescription: "Payment Description.",
                                         incomeSplitConfig: [],
   			                 metaData: ["deviceType":"ios", "userId":"user314285714"],
                                         paymentMethods: [PaymentMethod.card, PaymentMethod.accountTransfer],
                                         tokeniseCard: false)
```

5. ##### Launch the payment gateway when 'Pay button' is clicked.
   This is done on the initializePayment() method which requires the following:

    * A ViewController instance.    

    * An instance of TransactionParameters, e.g `parameters` above.

    * A completion/callback to be called with the monnify SDK is completing either after a failure or success.
```
   monnify.initializePayment(withTransactionParameters: parameter,
                          presentingViewController: self,
                          onTransactionSuccessful: { result in
    print(" Result \(result) ")
})
```



6. ##### Get the outcome of the payment attempt
    Finally, you should get the outcome of the transaction after completion.
    The result is available via the callback registered in `initializePayment()` as described above.

   For example, you can access the transaction status,transaction reference, payment reference, payment method, and the amount paid from the result as shown below.
```
let transactionStatus = result.transactionStatus
```

___
For further information, you can check out our [developer documentation](https://developers.monnify.com) and you can also join our [slack community](https://slack.monnify.com).


