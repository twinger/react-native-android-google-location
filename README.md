
# react-native-android-google-location
Location acquisition through Google Play Services.

## Add Google Play Services to Your Project

To make the Google Play services APIs available to your app:

1. Open the build.gradle file inside your application module directory (app/build.gradle).

2. Add a new build rule under dependencies for the latest version of play-services, using one of the APIs listed below.
Ensure that your top-level build.gradle contains a reference to the google() repo or to maven { url "https://maven.google.com" }

3. Save the changes, and click Sync Project with Gradle Files in the toolbar.
Check [here](https://developers.google.com/android/guides/setup) 

## Getting started

`$ npm install react-native-android-google-location --save`

### Mostly automatic installation

`$ react-native link react-native-android-google-location`

### Manual installation


#### Android

1. Open up `android/app/src/main/java/[...]/MainActivity.java`
  - Add `import co.twinger.gglocation.RNAndroidGoogleLocationPackage;` to the imports at the top of the file
  - Add `new RNAndroidGoogleLocationPackage()` to the list returned by the `getPackages()` method
2. Append the following lines to `android/settings.gradle`:
  	```
  	include ':react-native-android-google-location'
  	project(':react-native-android-google-location').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-android-google-location/android')
  	```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
  	```
      compile project(':react-native-android-google-location')
  	```

#### Add Permissions and used Google API to your Project

Add this to your AndroidManifest file;

``` xml
// file: android/app/src/main/AndroidManifest.xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```
Make sure this goes at the bottom of the `<application>` tag.
``` xml
	<uses-library android:name="com.google.android.maps" />
	<meta-data
        android:name="com.google.android.gms.version"
        android:value="@integer/google_play_services_version" />
```

## Usage
```javascript
import RNAndroidGoogleLocation from 'react-native-android-google-location';


export default class RNGoogleLocationExample extends Component {
  constructor(props) {
    super(props);
    this.state = {
      lng: 0.0, 
      lat: 0.0,
    };

    if (!this.eventEmitter) {
      // Register Listener Callback - has to be removed later
      this.eventEmitter = DeviceEventEmitter.addListener('updateLocation', this.onLocationChange.bind(this));
      // Initialize RNGLocation
      RNAndroidGoogleLocation.getLocation();
    }
  }

  onLocationChange (e: Event) {
    this.setState({
      lng: e.Longitude, 
      lat: e.Latitude 
    });
  }

  componentWillUnmount() {
    // Stop listening for Events
    this.eventEmitter.remove();
  }

  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.location}>
          Lng: {this.state.lng} Lat: {this.state.lat}
        </Text>
      </View>
    );
  }
}

```
  