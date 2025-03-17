import React, { useState } from "react";
import { initializeApp } from "firebase/app";
import { getAuth, signInWithPhoneNumber, RecaptchaVerifier } from "firebase/auth";
import { Button, Card, Input } from "@/components/ui";

const firebaseConfig = {
  apiKey: "YOUR_FIREBASE_API_KEY",
  authDomain: "YOUR_FIREBASE_AUTH_DOMAIN",
  projectId: "YOUR_FIREBASE_PROJECT_ID",
  storageBucket: "YOUR_FIREBASE_STORAGE_BUCKET",
  messagingSenderId: "YOUR_FIREBASE_MESSAGING_SENDER_ID",
  appId: "YOUR_FIREBASE_APP_ID"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);

export default function GoutamLenses() {
  const [phone, setPhone] = useState("");
  const [otp, setOtp] = useState("");
  const [confirmationResult, setConfirmationResult] = useState(null);
  
  const sendOtp = async () => {
    window.recaptchaVerifier = new RecaptchaVerifier('recaptcha-container', {size: 'invisible'}, auth);
    const confirmation = await signInWithPhoneNumber(auth, phone, window.recaptchaVerifier);
    setConfirmationResult(confirmation);
  };
  
  const verifyOtp = async () => {
    await confirmationResult.confirm(otp);
    alert("Login Successful!");
  };

  return (
    <div className="flex flex-col items-center p-10 bg-gray-100 min-h-screen">
      <h1 className="text-3xl font-bold mb-6">Goutam Lenses</h1>
      <Card className="w-96 p-6">
        <h2 className="text-xl mb-4">Login with Phone</h2>
        <Input placeholder="Enter phone number" value={phone} onChange={(e) => setPhone(e.target.value)} className="mb-4"/>
        <Button onClick={sendOtp}>Send OTP</Button>
        {confirmationResult && (
          <>
            <Input placeholder="Enter OTP" value={otp} onChange={(e) => setOtp(e.target.value)} className="mt-4 mb-2"/>
            <Button onClick={verifyOtp}>Verify OTP</Button>
          </>
        )}
        <div id="recaptcha-container"></div>
      </Card>
    </div>
  );
}
