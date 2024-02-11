# rexpay

This is a JavaScript library for implementing XpressPay payment gateway

## Demo

![Demo](rexpay.png?raw=true "Demo Image")

## Get Started

This Javascript library provides a wrapper to add RexPay to your React application


### Install

```sh
npm install i rexpay
```

or with `yarn`

```sh
yarn add rexpay
```

### Usage

This library can be implemented into any Javascript framework application


### 1. Using React

```javascript
import logo from "./logo.svg";
import "./App.css";
import React, { useState, useEffect } from "react";
import RexPay from "rexpay";

function App() {
  const [state, setState] = useState({
    amount: "",
    loading: false,
    transactions: [],
  });
  function OnClickPayButton() {
    let transactionId = "Test" + Math.floor(Math.random() * 1000000);
    setState({ ...state, loading: true });
    const rex = new RexPay();
    try {
      rex.initializePayment({
        reference: transactionId,
        amount: 100,
        currency: "NGN",
        userId: "test@gmail.com",
        callbackUrl: "google.com",
        metadata: {
          email: "test@gmail.com",
          customerName: "Test User",
        },
      }).then((response) => {
        if (response.success) {
          setState({ ...state, loading: false });
          sessionStorage.setItem("tranId", transactionId); // it can be saved to Database.
          sessionStorage.setItem("reference", response.data?.reference); // it can be saved to Database
          window.location.href = response.data?.authorizeUrl;
        } else {
          setState({ ...state, loading: false });
          window.location.href = response.data?.authorizeUrl;
        }
      });
    } catch (error) {
      //handle error
      console.log(error);
    }
  }

  function VerifyPayment() {
    try {
      const tranId =
        localStorage.getItem("tranId") === null
          ? ""
          : localStorage.getItem("tranId");
     const rex = new RexPay();
      rex.VerifyPayment({
        transactionReference: tranId,
      }).then((response) => {
        let amount = response?.data?.amount;
        if (amount) {
          setState({ ...state, amount, transactions: response.data.history });
        } else {
          setState({ ...state, amount: "" });
        }
      });
    } catch (error) {
      //handle error
      setState({ ...state, amount: "" });
    }
  }
  useEffect(() => {
    // or ComponentDidMount if you are using class component
    VerifyPayment();
  }, []);

  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        {state.amount?.length > 0 ? (
          <p>
            You have paid <code>{state.amount}</code>
          </p>
        ) : (
          ""
        )}
        <button
          onClick={() => OnClickPayButton()}
          style={{
            backgroundColor: state.loading ? "#afdbb1" : "",
            height: "30px",
            width: "120px",
            borderRadius: "5px",
            background: "#3cbe3c",
            border: "none",
            color: "white",
            fontWeight: "bold",
            cursor: "pointer",
          }}
          disabled={state.loading ? true : false}
          type="submit"
        >
          {state.loading ? "Paying" : " Pay 1,000"}
        </button>
      </header>
    </div>
  );
}


```
  #### Request for calling InitialisePayment function.

To initialize the transaction, you'll need to pass information such as email, first name, last name amount, publicKey, etc. Email and amount are required. You can also pass any other additional information in the metadata object field. Here is the full list of parameters you can pass:
|Param       | Type                 | Default    | Required | Description                      
| :------------ | :------------------- | :--------- | :------- | :-------------------------------------------------
| amount	| `number`			   | undefined      | `true`  | Amount you want to debit customer e.g 1000.00, 10.00...
| reference      | `string`             | undefined   | `true`  | Unique case sensitive transaction identification
| userId | `string`             | undefined       | `true`  | Email address of customer or any user identification
| publicKey       | `string`        | undefined | `true`  | Your public key from XpressPay.
| currency      | `string`  |  `NGN`    | `true`   | Currency charge should be performed in. Allowed only `NGN`.
| mode      | `string`  |  `Debug`    | `true`   | Allowed values are `Debug` or `Live`.
| callBackUrl      | `string`  |  your current url page    | `false`   | CallbackUrl is the url you want your customer to be redirected to when payment is successful. The default url is the page url where customer intialized payment.
| metadata      | `object`  |  empty `object`    | `false`   | Object containing any extra information you want recorded with the transaction.

Please checkout [RexPay Documentation](https://github.com) other ways you can integrate with our plugin

## How can I thank you?

Why not star the github repo? I'd love the attention! Why not share the link for this repository on Twitter or Any Social Media? Spread the word!

Don't forget to [follow me on twitter](https://twitter.com/muyiTechBadtGuy)!

Thanks!
Olumuyiwa Aro.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
