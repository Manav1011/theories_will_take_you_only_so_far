# ADB devices

adb kill-server
adb start-server

adb tcpip 5555

adb connect your_device_ip_address:5555

adb disconnect your_device_ip_address:5555

# React native storage mechanisms


In React Native, besides `AsyncStorage`, there are other types of storage options that you can use based on your specific needs. Here are some common storage solutions in React Native:

1. **Secure Storage:**

   - For storing sensitive information, such as authentication tokens or API keys, you may want to use secure storage solutions.
   - On iOS, you can use the Keychain for secure storage. The `react-native-keychain` library provides a React Native interface for interacting with the Keychain.
   - On Android, you can use the `react-native-sensitive-info` library to securely store sensitive information.
2. **SQLite Databases:**

   - If your application requires a relational database, you can use SQLite in React Native. The `react-native-sqlite-storage` library provides an interface for using SQLite databases in your React Native app.
   - SQLite allows you to perform structured queries and transactions, making it suitable for more complex data storage scenarios.
3. **Realm Database:**

   - Realm is a mobile database that is an alternative to SQLite. It is designed to be fast and efficient for mobile applications.
   - The `realm` library in React Native provides a JavaScript interface to interact with Realm databases.
4. **File System:**

   - React Native provides access to the device's file system, allowing you to store and retrieve files.
   - You can use the `react-native-fs` library to interact with the file system and perform operations such as reading and writing files.
5. **Redux-Persist:**

   - If you are using Redux for state management, `redux-persist` is a library that integrates with Redux to persist and rehydrate your Redux store across app restarts.
   - It supports various storage backends, including `AsyncStorage`, to store the persisted state.
6. **Firebase Realtime Database / Firestore:**

   - If you prefer cloud-based storage, Firebase Realtime Database or Firestore can be used in React Native applications.
   - The `react-native-firebase` library provides a React Native interface for Firebase services, including the Realtime Database and Firestore.
7. **Apollo Client Cache:**

   - If your app is using Apollo Client for GraphQL, you can utilize the local cache provided by Apollo Client to persist and rehydrate data between app sessions
