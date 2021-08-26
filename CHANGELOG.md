# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

<a name="v6.0.2"></a>
 ## v6.0.2 (2021-08-26)
### Changed
 * Security improvements

<a name="v6.0.1"></a>
 ## v6.0.1 (2021-05-19)
 ### Changed
 * Improved error log
 * Fixed crash when checking pinpad connection and pinpad is off

<a name="v6.0.0"></a>
## v6.0.0 (2021-04-01)
### Added
* **New `MoipMpos.searchPairedPinpads(PairedPinpadsCallback)` method** - Method that allows you to search for bluetooth pinpads paired to user's smartphone or tablet.
* **New `MoipMpos.isPinpadConnected(BluetoothDevice, PinpadCallback)` method** - Method that allows you to check if a pinpad is connected.
* **New `MoipMpos.charge(Context, BluetoothDevice, MposPaymentRequest, MposCallback)` method** - Method that allows you to create a mPOS charge using an specific paired pinpad.
### Changed
* **Method `MoipMpos.isPinpadConnected(PinpadCallback)` became deprecated** - This method will no longer work in future versions.
* **Method `MoipMpos.charge(Context, MposPaymentRequest, MposCallback)` became deprecated** - This method will no longer work in future versions.

<a name="v5.4.0"></a>
## v5.4.0 (2019-05-14)
### Added
* Now you can send additional informations of customer on `MposPaymentRequest`

<a name="v5.0.1"></a>
## v5.0.1 (2019-04-15)
### Added
* Now it is possible to transact with Amex Cards.
* Transactional flow is now safer.
* Fixed some bugs.

<a name="v4.1.0"></a>
## v4.1.0 (2018-01-04)
### Fixed
* **Fixed connection errors** - Fixed some possible connection errors.

<a name="v4.0.9"></a>
## v4.0.9 (2018-12-29)
### Added
* **New `searchDevices` method** - Added a method that allows you to search for discoverable bluetooth pinpads, you need to . (The user needs to press `0` so the pinpad is discoverable)  
* **New `PinpadSearchCallback` interface** - This interface returns either a BluetoothDevice on `onDeviceFound` or a MposError on `onError` callback.
* **New `stopSearch` method** - Added new method to stop the broadcast receiver from the `searchDevices` method
* **New `pairDevice` method** - Added new method to pair a device from inside your app, call this method sending the `bluetoothDevice` returned at the `searchDevices` method.

<a name="v4.0.8"></a>
## v4.0.8 (2018-11-29)
### Fixed
* **Fixed Receiver Request method** - Fixed method setReceiverRequest.

<a name="v4.0.7"></a>
## v4.0.7 (2018-11-21)
### Fixed
* **Fixed bluetooth connection** - Fixed bluetooth connection with some specific scenarios and devices.

<a name="v4.0.6"></a>
## v4.0.6 (2018-10-08)
### Added
* **New `isPinpadConnected` method** - Added new method that checks if there's a pinpad connection or not
### Fixed
* **Changed `MposPaymentRequest.setEmail()` signature** - Changed object to use the same signature to complain with method chaining as the rest of the object. Changed `mposPaymentRequest.setEmail(string)` to `mposPaymentRequest.email(string)` 
* **Fixed incorrect return of `MposAction.PINPAD_FOUND**`- Fixed return of `PINPAD_FOUND`in situations where the pinpad was previously paired but not powered on.
<a name="v4.0.5"></a>
## v4.0.5 (2018-09-11)
### IMPORTANT CHANGE!
* Changed the acquirer communication, using a version prior to this can cause low payment conversion rate. 

<a name="v4.0.4"></a>
## v4.0.4 (2018-09-11)
### Added
* **New Status to MposAction** - Added `UPDATING_POS_TABLES` status to notify when pinpad is uploading its tables.
* **Error callback when cancel button is pressed** - When the user press the cancel button on the pinpad a `POS_OPERATION_CANCELLED_BY_USER` will be returned on `onError` callback
* **Error callback when card is removed** - If the card is removed a `GENERIC_ERROR` will be returned on `onError` callback. **IMPORTANT:** The generic error is being returned due to a limitation of the current sdk/pinpad version, we intent to return a specific error for this action as soon as the pinpad connection sdk is updated.

<a name="v4.0.3"></a>
## v4.0.3 (2018-09-11)
### Added
* **Fix PaymentType bug** Fixed a bug that was making `MposPaymentResponse` `paymentType` and `Installment` to be returned with wrong value

<a name="v4.0.2"></a>
## v4.0.2 (2018-09-10)
### Added
* **Fix GPS bug** fixed a bug that was preventing the errors caused by geolocation services to not be returned on `onError` method

<a name="v4.0.1"></a>
## v4.0.1 (2018-09-04)
### Added
* **Fix GPS bug** Fixed a bug when location service is disabled
