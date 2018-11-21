# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

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
