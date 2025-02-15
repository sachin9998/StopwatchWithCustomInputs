# Stopwatch with Custom Inputs - React Machine Coding

<div align="center">
 <img src="https://github.com/user-attachments/assets/01eadc32-b344-458d-bb9f-7fc780f9d8ec" />
</div>



**Logic:**

1. We are managing time to store the user input time.
2. Will convert input time into total seconds.
3. When counter starts we are updating the Input values with useEffect.

**Code:**

```jsx
import { useEffect, useState } from "react";

const Countdown = () => {
  const [time, setTime] = useState({
    hour: 0,
    minute: 0,
    second: 0,
  });

  const [isRunning, setIsRunning] = useState(false);
  const [totalSeconds, setTotalSeconds] = useState(0);

  const handleStartStop = () => {
    if (!isRunning) {
      const total = time.hour * 3600 + time.minute * 60 + time.second;

      if (total <= 0) return;

      setTotalSeconds(total);
    }
    setIsRunning((prev) => !prev);
  };

  useEffect(() => {
    let timer;
    if (isRunning && totalSeconds > 0) {
      timer = setInterval(() => {
        setTotalSeconds((prev) => prev - 1);
      }, 1000);
    }

    return () => clearInterval(timer);
  }, [isRunning, totalSeconds]);

  useEffect(() => {
    const hours = Math.floor(totalSeconds / 3600);
    const minutes = Math.floor((totalSeconds % 3600) / 60);
    const seconds = totalSeconds % 60;

    setTime({ hour: hours, minute: minutes, second: seconds });
  }, [totalSeconds]);

  const handleChange = (e, field) => {
    let value = parseInt(e.target.value) || 0; // Convert to number or default to 0

    if (value < 0) value = 0; // Prevent negative values

    // Setting time inputes
    let newTime = { ...time, [field]: value };

    // Adjust minutes & seconds
    newTime.minute += Math.floor(newTime.second / 60);
    newTime.second %= 60;
    newTime.hour += Math.floor(newTime.minute / 60);
    newTime.minute %= 60;

    setTime(newTime);
  };

  const handleReset = () => {
    setIsRunning(false);
    setTime({ hour: 0, minute: 0, second: 0 });
    setTotalSeconds(0);
  };

  return (
    <div className="bg-white shadow-xl rounded-xl p-8 w-96 text-center">
      <h1 className="text-3xl font-semibold text-gray-800 mb-6">
        Countdown Timer
      </h1>

      <div className="flex justify-center items-center gap-4 mb-6">
        <input
          className="w-16 h-12 border border-gray-300 rounded-lg text-center text-xl font-semibold outline-none focus:ring-1 focus:ring-blue-500 focus:border-none"
          type="number"
          placeholder="HH"
          value={time.hour}
          onChange={(e) => {
            handleChange(e, "hour");
          }}
        />
        <span className="text-2xl font-bold">:</span>
        <input
          className="w-16 h-12 border border-gray-300 rounded-lg text-center text-xl font-semibold outline-none focus:ring-1 focus:ring-blue-500 focus:border-none"
          type="number"
          placeholder="MM"
          value={time.minute}
          onChange={(e) => {
            handleChange(e, "minute");
          }}
        />
        <span className="text-2xl font-bold">:</span>
        <input
          className="w-16 h-12 border border-gray-300 rounded-lg text-center text-xl font-semibold outline-none focus:ring-1 focus:ring-blue-500 focus:border-none"
          type="number"
          placeholder="SS"
          value={time.second}
          onChange={(e) => {
            handleChange(e, "second");
          }}
        />
      </div>

      <div className="flex justify-center gap-4">
        <button
          onClick={handleStartStop}
          className="bg-blue-600 text-white px-6 py-2 rounded-lg text-lg font-semibold hover:bg-blue-700 transition"
        >
          {isRunning ? "Stop" : "Start"}
        </button>
        <button
          onClick={handleReset}
          className="bg-red-500 text-white px-6 py-2 rounded-lg text-lg font-semibold hover:bg-red-600 transition"
        >
          Reset
        </button>
      </div>
    </div>
  );
};

export default Countdown;

```
