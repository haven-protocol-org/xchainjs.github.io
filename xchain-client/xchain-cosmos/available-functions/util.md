# Util

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

## DECIMAL

The decimal for cosmos chain.

## isMsgSend

Type guard for MsgSend

### Parameters

-   `msg` **Msg** 

Returns **[boolean][1]** `true` or `false`.

## isMsgMultiSend

Type guard for MsgMultiSend

### Parameters

-   `msg` **Msg** 

Returns **[boolean][1]** `true` or `false`.

## getDenom

Get denomination from Asset

### Parameters

-   `asset` **Asset** 

Returns **[string][2]** The denomination of the given asset.

## getAsset

Get Asset from denomination

### Parameters

-   `denom` **[string][2]** 

Returns **(Asset \| null)** The asset of the given denomination.

## getTxsFromHistory

Parse transaction type

### Parameters

-   `txs` **[Array][3]&lt;TxResponse>** The transaction response from the node.
-   `mainAsset` **Asset** Current main asset which depends on the network.

Returns **[Array][3]&lt;Tx>** The parsed transaction result.

## getQueryString

Get Query String

### Parameters

-   `params`  
-   `APIQueryParam`  

Returns **[string][2]** The query string.

## getDefaultFees

Get the default fee.

Returns **Fees** The default fee.

## getPrefix

Get address prefix based on the network.

Returns **[string][2]** The address prefix based on the network.\*

[1]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean

[2]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String

[3]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array
