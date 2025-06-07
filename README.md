# Aviacoin
Симулятор авиатора
import { useState, useEffect } from "react"; import { Button } from "@/components/ui/button"; import { Card, CardContent } from "@/components/ui/card";

export default function AviacoinSimulator() { const [balance, setBalance] = useState(1000); const [bet, setBet] = useState(0); const [multiplier, setMultiplier] = useState(1); const [gameRunning, setGameRunning] = useState(false); const [crashPoint, setCrashPoint] = useState(0); const [cashedOut, setCashedOut] = useState(false);

useEffect(() => { let interval; if (gameRunning && multiplier < crashPoint && !cashedOut) { interval = setInterval(() => { setMultiplier((prev) => parseFloat((prev + 0.01).toFixed(2))); }, 100); } else if (multiplier >= crashPoint) { endGame(false); } return () => clearInterval(interval); }, [gameRunning, multiplier, crashPoint, cashedOut]);

const startGame = () => { if (bet <= 0 || bet > balance) return; setGameRunning(true); setCrashPoint(parseFloat((Math.random() * 4 + 1).toFixed(2))); setMultiplier(1); setCashedOut(false); setBalance((prev) => prev - bet); };

const cashOut = () => { if (gameRunning && !cashedOut) { const winnings = parseFloat((bet * multiplier).toFixed(2)); setBalance((prev) => prev + winnings); setCashedOut(true); endGame(true); } };

const endGame = (cashed) => { setTimeout(() => { setGameRunning(false); setMultiplier(1); setCrashPoint(0); setBet(0); setCashedOut(false); }, 1500); };

return ( <div className="p-4 max-w-md mx-auto"> <Card className="mb-4"> <CardContent> <div className="text-xl font-bold mb-2">Баланс: {balance.toFixed(2)} 🪙 Aviacoins</div> <input type="number" placeholder="Ставка" value={bet} disabled={gameRunning} onChange={(e) => setBet(Number(e.target.value))} className="p-2 border rounded w-full mb-2" /> <div className="text-lg mb-2">x{multiplier.toFixed(2)}</div> <div className="flex gap-2"> <Button onClick={startGame} disabled={gameRunning}>Старт</Button> <Button onClick={cashOut} disabled={!gameRunning || cashedOut}>Вывести</Button> </div> </CardContent> </Card> </div> ); }

