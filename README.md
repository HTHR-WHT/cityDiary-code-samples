# City Diary - code samples

<h3>
City Diary is a mobile app that allows you to interact with and explore a user generated sound map.
</h3>

Here is a [link to the City Diary repo](https://github.com/HCJM/City-Diary)

Watch our [City Diary demo video here](https://www.youtube.com/watch?v=GZ4RHsHUgW8&list=PLx0iOsdUOUmmCV8l-qiwghVSCpL7kQiWJ&index=4)!

<h4>
The individual contributions that I would like to share are focused in the team's PublicMapScreen component.  Originally this was all located in one component file, but as the work progressed we extracted out some of the logic into a seperate file for modularity and organization.  I will be sharing both files below with a list highlighting my individual contributions to the team.
</h4>

---
[City Diary - Public Map Screen Component File](https://github.com/HCJM/City-Diary/blob/main/src/screens/PublicMapScreen/PublicMapScreen.js)

- researched, installed and implemented ```expo-location``` module _version 14.0.1_ - Expo Location allows reading geolocation information from the device. Your app can poll for the current location or subscribe to location update events.

- researched, installed and implemented ```react-native-maps``` module _version 0.29.4_ - React Native Maps provides a Map component that uses Apple Maps or Google Maps on iOS and Google Maps on Android.

- *lines 29-30:* research and implement global user context by coding custom authentication hook
```
// currentUser is an object with these properties: email, firstName, id, lastName, userName
const currentUser = useAuth().currentUser || {}
```
- *lines 14-18:* set up deltas for consistent zoom in map view
```
// deltas control how much of the map to display. The amount of 'zoom'.
const deltas = {
  latitudeDelta: 0.2,
  longitudeDelta: 0.05,
}
```
- *line 42:* set up default NYC region for guests and logged out users
```
const defaultRegionNYC = { latitude: 40.73061, longitude: -73.97, ...deltas }
```
- *lines 52-59:* implemented Location request method to access current user region and set state
```
let location = await Location.getCurrentPositionAsync({})
      setLocation(location)
      const myRegion = {
        latitude: location.coords.latitude,
        longitude: location.coords.longitude,
        ...deltas,
      }
      setUserRegion(myRegion)
```
---
[City Diary - Map Screen Module Component File](https://github.com/HCJM/City-Diary/blob/main/src/screens/PublicMapScreen/MapScreenModule.js)

- *lines 23-31:* coded functions for retrieving firebase audio documents with privacy filtering
```
const filterOutAllPrivateAudio = audioDetails.filter(
    (audioDoc) => audioDoc.data.isPrivate === false
  )

  const filterOutOthersPrivateAudio = audioDetails.filter(
    (audioDoc) =>
      audioDoc.data.userId === currentUser.id ||
      audioDoc.data.isPrivate === false
  )
```

- *lines 71-153:* implemented filtering logic based on current user authentication
```
{currentUser.id
          ? filterOutOthersPrivateAudio.map((audioDoc) => (
              <Marker 
              *...code omitted for conciseness* 
              >
          : filterOutAllPrivateAudio.map((audioDoc) => (
              <Marker
              *...code omitted for conciseness* 
              >
```
- *lines 86-92:* coded Marker ```pinColors``` logic to visually differentiate between public, user public, and user private audio documents
```
pinColor={
    audioDoc.data.userId === currentUser.id
    ? audioDoc.data.isPrivate
        ? '#306B34'
        : '#FCAF58'
        : '#FF5A5F'
}
```
