import React, { useState } from "react";
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { ShoppingCart } from "lucide-react";

const products = [
  { id: 1, name: "Aviator Sunglasses", price: 1499, image: "/aviator.jpg" },
  { id: 2, name: "Wayfarer Sunglasses", price: 1299, image: "/wayfarer.jpg" },
  { id: 3, name: "Round Sunglasses", price: 999, image: "/round.jpg" },
  { id: 4, name: "Clubmaster Sunglasses", price: 1599, image: "/clubmaster.jpg" },
  { id: 5, name: "Oversized Sunglasses", price: 1799, image: "/oversized.jpg" },
  { id: 6, name: "Sport Sunglasses", price: 1999, image: "/sport.jpg" },
];

function Home() {
  return (
    <div className="p-6 text-center">
      <h1 className="text-4xl font-bold mb-6">Welcome to Goutam Lenses</h1>
      <p className="text-lg mb-6">Find the best sunglasses for every occasion.</p>
      <Link to="/shop">
        <Button className="px-6 py-2 text-lg">Shop Now</Button>
      </Link>
    </div>
  );
}

function Shop() {
  return (
    <div className="p-6">
      <h1 className="text-3xl font-bold text-center mb-6">Goutam Lenses - Shop</h1>
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
        {products.map((product) => (
          <Card key={product.id} className="p-4 shadow-lg rounded-lg">
            <img src={product.image} alt={product.name} className="w-full h-48 object-cover mb-4 rounded-md" />
            <CardContent>
              <h2 className="text-xl font-semibold">{product.name}</h2>
              <p className="text-gray-600">₹{product.price}</p>
              <Button className="mt-4 w-full flex items-center gap-2">
                <ShoppingCart size={18} /> <span>Add to Cart</span>
              </Button>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  );
}

function Login() {
  const [mobile, setMobile] = useState("");
  const [otpSent, setOtpSent] = useState(false);
  const [otp, setOtp] = useState("");
  const [loggedIn, setLoggedIn] = useState(false);

  const sendOtp = () => {
    if (mobile.length === 10) {
      setOtpSent(true);
      alert("OTP sent to " + mobile);
    } else {
      alert("Enter a valid 10-digit mobile number");
    }
  };

  const verifyOtp = () => {
    if (otp.length === 6) {
      setLoggedIn(true);
      alert("Login Successful");
    } else {
      alert("Invalid OTP");
    }
  };

  return (
    <div className="p-6 text-center">
      <h1 className="text-3xl font-bold mb-6">Login with OTP</h1>
      {!loggedIn ? (
        !otpSent ? (
          <>
            <input type="tel" placeholder="Enter Mobile Number" value={mobile} onChange={(e) => setMobile(e.target.value)} className="p-2 border rounded mb-4 w-full" />
            <Button className="w-full" onClick={sendOtp}>Send OTP</Button>
          </>
        ) : (
          <>
            <input type="text" placeholder="Enter OTP" value={otp} onChange={(e) => setOtp(e.target.value)} className="p-2 border rounded mb-4 w-full" />
            <Button className="w-full" onClick={verifyOtp}>Verify OTP</Button>
          </>
        )
      ) : (
        <div>
          <h2 className="text-2xl font-semibold">Welcome, User</h2>
          <p className="text-gray-600">Mobile: {mobile}</p>
          <Button className="mt-4" onClick={() => setLoggedIn(false)}>Logout</Button>
        </div>
      )}
    </div>
  );
}

export default function App() {
  return (
    <Router>
      <nav className="p-4 bg-gray-200 flex justify-between">
        <Link to="/" className="text-xl font-bold">Goutam Lenses</Link>
        <div>
          <Link to="/shop" className="mr-4">Shop</Link>
          <Link to="/login">Login</Link>
        </div>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/shop" element={<Shop />} />
        <Route path="/login" element={<Login />} />
      </Routes>
    </Router>
  );
}
