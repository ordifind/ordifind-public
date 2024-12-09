# OIFI Wallet API Documentation

The `oifi` wallet API allows developers to interact with a user's Dogecoin wallet through various functionalities, such as connecting to the wallet, retrieving account details, managing balances, signing PSBTs, and sending transactions.

## Global `window.oifi` Object

The `window.oifi` object is the entry point for all wallet API functionalities. Below are the available methods and their usage.

---

## Interfaces

### `walletResponse<T>`

A generic interface for responses returned by the wallet API methods.

#### Properties

- **`status`:** `"success"` | `"faild"`  
  Indicates whether the operation succeeded or failed.
- **`response`:** `T`  
  The response payload of type `T` (varies by method).
- **`reason?`:** `string`  
  Optional error message if the operation failed.

#### Example

```typescript
interface walletResponse<T> {
  status: "success" | "faild";
  response: T;
  reason?: string;
}
```

### `interface options`

Represents options for signing PSBTs.

- **Properties:**
  - `finalize`: `boolean` - Indicates whether to finalize the transaction.
  - `inputToSign`: Array of inputs to sign, each containing:
    - `inputIndex`: `number` - Index of the input.
    - `SignHashType`: `number[]` - Array of hash types.

#### Example

```typescript
interface options {
  finalize: boolean;
  inputToSign: { inputIndex: number; SignHashType: number[] }[];
}
```

### Methods

#### `connect(): Promise<walletResponse<{ address: string; balance: number }>>`

Prompts the user to connect their wallet.

- **Returns:**
  - A `Promise` that resolves to a `walletResponse` object containing:
    - `address`: The connected wallet address.
    - `balance`: The current balance in the wallet.

---

#### `getAccount(): Promise<walletResponse<{ Account: string }>>`

Retrieves the user's connected wallet account.

- **Returns:**
  - A `Promise` that resolves to a `walletResponse` object containing:
    - `Account`: The user's wallet account address.

---

#### `getBalance(): Promise<walletResponse<{ Balance: number }>>`

Fetches the balance of the connected wallet account.

- **Returns:**
  - A `Promise` that resolves to a `walletResponse` object containing:
    - `Balance`: The balance in the wallet.

---

#### `sendDogecoin(payload: { amount: number; gas: number; address: string }): Promise<walletResponse<{ txid: string }>>`

Sends Dogecoin to a specified address.

- **Parameters:**

  - `payload`: An object with the following properties:
    - `amount`: The amount of Dogecoin to send.
    - `gas`: The gas fee for the transaction.
    - `address`: The recipient's wallet address.

- **Returns:**
  - A `Promise` that resolves to a `walletResponse` object containing:
    - `txid`: The transaction ID of the completed transaction.

---

#### `signPSBT(payload: { psbt: string; options: options }): Promise<walletResponse<{ hex: string }>>`

Signs a PSBT (Partially Signed Bitcoin Transaction) using the wallet.

- **Parameters:**

  - `payload`: An object with the following properties:
    - `psbt`: The PSBT in base64 format.
    - `options`: Options for signing the PSBT.

- **Returns:**
  - A `Promise` that resolves to a `walletResponse` object containing:
    - `hex`: The signed PSBT in hexadecimal format.

---

#### `signPSBTs(payload: { psbt: string; options: options }[]): Promise<walletResponse<{ hex: { old: string; new: string }[] }>>`

Signs multiple PSBTs in a single operation.

- **Parameters:**

  - `payload`: An array of objects, each containing:
    - `psbt`: The PSBT to be signed in base64 format.
    - `options`: Options for signing the PSBT.

- **Returns:**
  - A `Promise` that resolves to a `walletResponse` object containing:
    - `hex`: An array of objects with `old` and `new` hexadecimal PSBT formats.

---

#### `signMessage(payload: { message: string }): Promise<walletResponse<{ signature: string }>>`

Signs an arbitrary message using the wallet.

- **Parameters:**

  - `payload`: An object containing:
    - `message`: The message to be signed.

- **Returns:**
  - A `Promise` that resolves to a `walletResponse` object containing:
    - `signature`: The signature of the message.

---

## Example Usage

### Connect Wallet

```typescript
if (window.oifi) {
  window.oifi
    .connect()
    .then((response) => {
      if (response.status === "success") {
        console.log("Wallet connected at address:", response.response.address);
      } else {
        console.error("Failed to connect:", response.reason);
      }
    })
    .catch((error) => console.error("Error connecting wallet:", error));
} else {
  console.error("Wallet API is not available");
}
```
