
# Managing React Native Build: Stop and Redeploy

This guide explains how to stop a React Native development server and redeploy your build on Android or iOS devices.

---

## 1. **Stopping Metro Bundler**

If the Metro Bundler is already running, you can stop it by:

- Pressing `Ctrl + C` in the terminal where Metro is running.
- Alternatively, terminate the process via your system's task manager if necessary.

---

## 2. **Clearing Previous Builds**

Before redeploying, it's often useful to clean up any previous builds to ensure a fresh deployment:

### For Android:
Run the following command to clean the Android build cache:
```bash
cd android
./gradlew clean
cd ..
```

### For iOS:
Clear the iOS build cache with:
```bash
cd ios
xcodebuild clean
cd ..
```

---

## 3. **Redeploying the Build**

### **Start Metro Bundler**
Start the Metro Bundler if it's not already running:
```bash
npm start
```

### **Deploy to Android**
To redeploy the app on an Android device or emulator, run:
```bash
npm run android
```

### **Deploy to iOS**
To redeploy the app on an iOS simulator or device, run:
```bash
npm run ios
```

---

## 4. **Hot Reloading and Live Reloading**

Once your app is running, you can enable the following features for faster development:
- **Hot Reloading**: Updates only the changed modules without restarting the app.
- **Live Reloading**: Reloads the app entirely when code changes.

These options can be toggled in the in-app developer menu (shake the device or press `Cmd + D` on iOS, `Cmd + M` on Android).

---

## Summary

To stop and redeploy a React Native app:
1. Stop Metro Bundler (`Ctrl + C`).
2. Clear previous builds (`./gradlew clean` for Android, `xcodebuild clean` for iOS).
3. Start Metro Bundler and redeploy using `npm run android` or `npm run ios`.

This ensures a clean, updated build for development.

---

Happy Coding!
