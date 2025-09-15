# Defib-Sim

Zoll X Series SimulatorThis is a high-fidelity web-based simulator for the Zoll X Series monitor/defibrillator, designed for medical training and educational purposes.FeaturesRealistic UI: The interface is modeled directly from photos and user manuals of the real device, including screen layouts, button designs, and color schemes.Dynamic Waveforms: Real-time ECG waveform generation for various cardiac rhythms (Normal Sinus, V-Fib, V-Tach, Asystole) drawn on an HTML5 canvas.Functional Controls: Core device functions are simulated, including:Energy selectionCharging and discharging (defibrillation)Rhythm analysis (AED advisory logic)Pacing mode activationNIBP measurement cycleReal-time Remote Control: The simulator is powered by Firebase Firestore, allowing an instructor to control the "patient's" state from a separate device in real-time. This enables dynamic and interactive training scenarios.How to Use the Remote Control FeatureThis simulator is designed to be a "monitor" that displays the patient's condition. A separate "controller" application (which can be as simple as the Firebase console or a dedicated webpage) is used to change the patient's state.Firebase Setup:You must create a free Firebase project at firebase.google.com.Enable Firestore Database in your project.Enable Anonymous Authentication in the Authentication section.Copy your project's configuration object (apiKey, authDomain, etc.) from the project settings.Paste this configuration into the firebaseConfig object at the top of the <script type="module"> tag in the zoll_x_series_simulator.html file.Starting a Simulation:Open the zoll_x_series_simulator.html file in a web browser.Enter a unique Session ID (e.g., scenario-1) in the connection modal. This ID links the monitor to the controller.Click "Connect & Start Simulation".Controlling the Simulation:Go to your Firebase project's Firestore Database console.You will see a collection named zoll-sim-sessions.Find the document with the ID you entered (e.g., scenario-1).You can now edit the fields in this document to control the simulator in real-time. For example:Change vitals.hr from 80 to 150.Change ecg.rhythm from 'NSR' to 'VF'.The simulator display will update instantly to reflect these changes.Data Structure for ControllerYour controller should read and write to a Firestore document at zoll-sim-sessions/{sessionId} with the following structure:{
  "vitals": {
    "hr": 80,
    "spo2": 97,
    "nibp": "121/79",
    "nibpMean": 96,
    "etco2": 38,
    "resp": 12,
    "temp": 98.6
  },
  "ecg": {
    "rhythm": "NSR", // "NSR", "VF", "VT", "Asystole"
    "lead": "II"
  },
  "deviceState": {
    "mode": "Monitor", // "Monitor", "AED", "ManualDefib", "Pacer"
    "defib": {
      "energy": 120,
      "status": "disarmed" // "disarmed", "charging", "charged", "shock_advised"
    },
    "pacer": {
      "mode": "Demand",
      "rate": 70,
      "output": 50,
      "active": false
    },
    "alarmsOn": true
  },
  "events": [
    // Events are logged here by the simulator
  ]
}
