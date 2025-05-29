# The-Information-Paradox-Raw-Code

s.freqscope;

(
SynthDef(\zarbeBam, {
	var sig, sum = 0, env;
	env = EnvGen.kr(Env.perc(0.1, 2), doneAction: 2);
	30.do{
		sig = SinOsc.ar(50 * {Rand(0.7, 4)} ! 2, {Rand(0.0, 1.0)} ! 2, env);
		sum = sum + sig;
	};
	sum = sum * 0.1;
	Out.ar(0, sum * 0.5);
}).add;

SynthDef.new(\start1, {
	arg freqs = #[369.99, 415.3, 523.25, 698.45, 783.99, 987.76, 1318.51, 1479.97, 1864.65, 2489.01, 2793.82, 3520.0], amps = #[1, 0.5, 0.3, 0.25, 0.2, 0.16, 0.14, 0.125, 0.11, 0.1, 0.09, 0.008], gate = 1;
	var sig, sum, env;
	sum = 0;
	env = EnvGen.kr(Env.new([0, 0.8, 0.7, 0.6, 0.5, 0.4, 0.3, 0.2, 0.5], [5, 1.1, 1.2, 1.3, 1.2, 1.1, 1, 1.8]), /*doneAction: 2*/);
	10.do{
		sig = SinOsc.ar(freqs * {Rand(0.99, 1.01)}, {Rand(0.0, 1.0)}, amps);
		sum = sum + sig;
	};
	sum = sum * env;
	sum = sum * 0.02;
	sum = Splay.ar(sum);
	Out.ar(0, sum);
}).add;

SynthDef.new(\start2, {
	arg freqs = #[509.92, 513.64, 516.75, 520, 523.58, 527, 529.92, 532.9, 538.73, 546.18, 620.91, 549.98, 656.06, 678.79, 692.49, 783.59, 790.8], amps = #[1, 0.5, 0.3, 0.25, 0.2, 0.16, 0.14, 0.125, 0.11, 0.1, 0.09, 0.008];
	var sig, sum, env, amp;
	sum = 0;
	env = EnvGen.kr(Env.new([0, 0.8, 0.7, 0.6, 0.5, 0.4, 0.3, 0.4], [5, 1.1, 1.2, 1.3, 1.2, 1.1, 1.8]));
	amp = LFDNoise0.kr(5).range(0.01, 0.9);
	10.do{
		sig = SinOsc.ar(freqs * {Rand(0.99, 1.01)}, {Rand(0.0, 1.0)}, amps);
		sum = sum + sig;
	};
	sum = sum * env * amp;
	sum = sum * 0.08;
	sum = Splay.ar(sum);
	Out.ar(0, sum);
}).add;

SynthDef.new(\raghs, {
	arg freqs = #[322, 646, 965, 1174, 1300, 1775, 1953, 2363, 2584, 2959, 3144, 4145, 6414, 8691], amps = #[0.9, 0.85, 0.5, 0.45, 0.7, 0.5, 0.4, 0.3, 0.2, 0.6, 0.3, 0.35, 0.3, 0.1];
	var sig, amp, sum;
	sum = 0;
	amp = LFDNoise0.kr(8).range(0.01, 0.9);
	10.do{
		sig = SinOsc.ar(freqs * {Rand(0.9, 1.1)}, {Rand(0.0, 1.0)}, amps);
		sum = sum + sig;
	};
	sum = Splay.ar(sum);
	sum = sum * amp;
	Out.ar(0, sum * 0.006);
}).add;

SynthDef.new(\bass, {
	arg freqs = #[50, 60, 70, 80, 90, 100, 150, 200],  amps = #[1, 0.9, 0.8, 0.7, 0.6, 0.5, 0.4, 0.3];
	var sig, env;
	env = SinOsc.kr(0.1).range(0.1, 1.0);
	sig = SinOsc.ar(freqs, 0.0, amps);
	sig = sig * env;
	sig = Splay.ar(sig);
	Out.ar(0, sig * 0.2);
}).add;
)

~raghs = Synth(\raghs);
~chord1 = Synth(\start1);
~chord2 = Synth(\start2);
~bass = Synth(\bass);
~zarbeBam = Synth(\zarbeBam);

(
Array.fill(20, {
	arg i;
	i + log10(i) * 200;     //disk1
}).round;
)

(
Array.fill(20, {
	arg i;
	i + log2(i) * 200;         //disk2
}).round;
)

(
Array.fill(20, {
	arg i;
	i + log2(i * 100) * 200;         //disk3
}).round;
)

(
Array.fill(25, {
	arg i;
	i * i ** 2;                    //disk4
});
)

(
Array.fill(25, {
	arg i;
	    100 * i;     //harmonics
}).round;
)

(
SynthDef.new(\disk1, {
	arg freq = #[200.0, 460.0, 695.0, 920.0, 1140.0, 1356.0, 1569.0, 1781.0, 1991.0, 2200.0, 2408.0, 2616.0, 2823.0, 3029.0, 3235.0, 3441.0, 3646.0, 3851.0, 4056.0];
	var sig, sum;
	sig = 0;
	10.do{
		sum = SinOsc.ar((freq * [Pulse.kr(1.1).range(0.95, 0.99), Pulse.kr(1.8).range(1.01, 1.05)]) * {Rand(0.99, 1.1)}, {Rand(0.0, 1.0)}, EnvGen.kr(Env.circle([1, 0], [MouseX.kr(0.7, 0.3), 0.01])));
		sig = sig + sum
	};
	sig = Splay.ar(sig);
	Out.ar(0, sig * 0.05);
}).add;

SynthDef.new(\disk2, {
	arg freq = #[250.0, 600.0, 917.0, 1200.0, 1464.0, 1717.0, 1961.0, 2200.0, 2434.0, 2664.0, 2892.0, 3117.0, 3340.0, 3561.0, 3781.0, 4000.0, 4217.0, 4434.0, 4650.0];
	var sig, sum;
	sig = 0;
	9.do{
		sum = SinOsc.ar((freq * [Pulse.kr(2).range(0.95, 0.99), Pulse.kr(2.2).range(1.01, 1.05)]) * {Rand(0.9, 1.01)}, {Rand(0.0, 1.0)}, EnvGen.kr(Env.circle([1, 0], [MouseX.kr(0.9, 0.5), 0.01])));
		sig = sig + sum;
	};
	sig = Splay.ar(sig);
	Out.ar(0, sig * 0.05);
}).add;

SynthDef.new(\disk3, {
	arg freq = #[1529.0, 1929.0, 2246.0, 2529.0, 2793.0, 3046.0, 3290.0, 3529.0, 3763.0, 3993.0, 4221.0, 4446.0, 4669.0, 4890.0, 5110.0, 5329.0, 5546.0, 5763.0, 5978.0];
	var sig;
	sig = SinOsc.ar(freq * [Pulse.kr(2).range(0.95, 0.99), Pulse.kr(2.2).range(1.01, 1.05)], {Rand(0.0, 1.0)}, EnvGen.kr(Env.circle([1, 0], [MouseX.kr(0.1, 1), 0.01])));
	sig = Splay.ar(sig);
	Out.ar(0, sig * 0.1);
}).add;

SynthDef.new(\disk4, {
	arg freq = #[81, 256, 625, 1296, 2401, 4096, 6561, 10000, 14641, 20736, 28561, 38416, 50625, 65536, 83521, 104976, 130321, 160000];
	var sig, sum;
	sig = 0;
	10.do{
		sum = SinOsc.ar((freq * [Pulse.kr(1.1).range(0.95, 0.9), Pulse.kr(1.8).range(1.01, 1.05)]) * {Rand(0.99, 1.1)}, {Rand(0.0, 1.0)}, EnvGen.kr(Env.circle([1, 0], [MouseX.kr(0.1, 0.01), 0.01])));
		sig = sig + sum
	};
	sig = Splay.ar(sig);
	Out.ar(0, sig * 0.05);
}).add;

SynthDef.new(\gliss1, {
	arg freq = #[60, 65, 70, 75, 80, 85, 90, 95, 100, 105, 110, 115, 120, 125, 130, 135, 140];
	var sig;
	sig = SinOsc.ar(EnvGen.kr(Env.circle([freq, freq * 2], [0.5, 0.5])), 0.0 );
	sig = Splay.ar(sig);
	Out.ar(0, sig * 0.2);
}).add;

SynthDef.new(\harmonics, {
	arg freq = #[100, 200, 300, 400, 500, 600, 700, 800, 900, 1000, 1100, 1200, 1300, 1400, 1500, 1600, 1700, 1800, 1900, 2000, 2100, 2200, 2300, 2400], amps = #[1.0, 0.5, 0.33, 0.25, 0.2, 0.16, 0.14, 0.125, 0.11, 0.1, 0.09, 0.083, 0.076, 0.071, 0.066, 0.0625, 0.058, 0.055, 0.052, 0.05, 0.047, 0.045, 0.043, 0.041, 0.04];
	var sig, sum, env;
	env = Pulse.kr(SinOsc.kr(1).range(1, 10)).range(1, 0.1);
	sum = 0;
	9.do{
		sig = SinOsc.ar(freq * {Rand(0.99, 1.1)}, {Rand(0.0, 1.0)}, amps);
		sum = sum + sig;
	};
	sum = Splay.ar(sum);
	sum = sum * env;
	Out.ar(0, sum * 0.5);
}).add;

SynthDef.new(\chord5, {
	arg freqs = #[1583, 1632, 1876, 1915, 2152, 2159, 2181, 2300, 3173, 3187, 3233, 3253, 3298, 3750, 3762, 3770, 3830, 4391, 4765, 4996, 6306, 6691, 7924, 8309, 3482.6, 3590.4, 4127.2, 4213.0, 4734.4, 4749.8, 4798.2, 5060.0, 6980.6, 7011.4, 7112.6, 7156.6, 7255.6, 8250.0, 8276.4, 8294.0, 8426.0, 9660.2, 10483.0, 10991.2, 13873.2, 14720.2, 17432.8, 18279.8],
	amp = #[0.2, 0.3, 0.4, 0.5, 0.6, 0.5, 1, 0.4, 0.3, 0.7, 0.1, 0.5];
	var sig, sum, env;
	env = EnvGen.kr(Env.perc(0.01, 2, 1), doneAction: 2);
	sum = 0;
	10.do{
		sig = SinOsc.ar(freqs * {Rand(0.99, 1.01)}, 0.0, amp);
		sum = sum + sig;
	};
	sum = Splay.ar(sum);
	sum = sum * env;
	Out.ar(0, sum * 0.1);
}).add;
)


~disk1 = Synth(\disk1);
~disk2 = Synth(\disk2);
~disk3 = Synth(\disk3);
~disk4 = Synth(\disk4);
~gliss1 = Synth(\gliss1);
~harmonics = Synth(\harmonics);
~chord5 = Synth(\chord5);

(
Array.fill(25, {
	arg i;
	i * i ** 2;
});
)


[1.0, 0.5, 0.33, 0.25, 0.2, 0.16, 0.14, 0.125, 0.11, 0.1, 0.09, 0.083, 0.076, 0.071, 0.066, 0.0625, 0.058, 0.055, 0.052, 0.05, 0.047, 0.045, 0.043, 0.041, 0.04]

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

(
SynthDef.new(\chordFinal, {
	arg freqs = #[65.25, 130.5, 261, 522, 783, 1044, 1305, 1566, 1827, 2088, 2349, 2610, 329.62, 659.24, 988.86, 1318.48, 1648.1, 1977.72, 2307.34, 2636.96, 2966.58, 3296.2, 391.99, 783.98, 1175.97, 1567.96, 1959.95, 2351.94, 2743.93, 3135.92, 3527.91, 3919.9, 493.88, 987.76, 1481.64, 1975.52, 2469.4, 2963.28, 3457.16, 3951.04, 4444.92, 4938.8, 8352, 587.32, 1174.64, 1761.96, 2349.28, 2936.6, 3523.92, 4111.24, 4698.56, 5285.88, 5873.2],
	amp = #[1.0, 1.0, 1.0, 0.5, 0.33, 0.25, 0.2, 0.16, 0.14, 0.125, 0.11, 0.1, 1.0, 0.5, 0.33, 0.25, 0.2, 0.16, 0.14, 0.125, 0.11, 0.1, 1.0, 0.5, 0.33, 0.25, 0.2, 0.16, 0.14, 0.125, 0.11, 0.1, 1.0, 0.5, 0.33, 0.25, 0.2, 0.16, 0.14, 0.125, 0.11, 0.1, 0.2, 1.0, 1.0, 1.0, 0.5, 0.33, 0.25, 0.2, 0.16, 0.14, 0.125, 0.11, 0.1];
	var sig, sum, env;
	env = EnvGen.kr(Env.adsr(0.5, 5, 0.5, 5, 0.7), doneAction: 2);
	sum = 0;
	10.do{
		sig = SinOsc.ar(freqs * {Rand(0.99, 1.01)}, 0.0, amp);
		sum = sum + sig;
	};
	sum = Splay.ar(sum);
	sum = sum * 0.2 * env;
	Out.ar(0, sum);
}).add;
)

~chordFinal = Synth(\chordFinal);


(
Array.fill(11, {
	arg i;
	[261.62, 329.62, 391.99] * i;
});
)

(60 + [0, 4, 7]).midicps

[261.62, 329.62, 391.99]

[261.62, 329.62, 391.99, 523.24, 659.24, 783.98, 784.86, 988.86, 1175.97, 1046.48, 1318.48, 1567.96, 1308.1, 1648.1, 1959.95, 1569.72, 1977.72, 2351.94, 1831.34, 2307.34, 2743.93, 2092.96, 2636.96, 3135.92, 2354.58, 2966.58, 3527.91, 2616.2, 3296.2, 3919.9]

(
SynthDef.new(\chrd, {
	arg freqs = #[261.62, 329.62, 391.99, 523.24, 659.24, 783.98, 784.86, 988.86, 1175.97, 1046.48, 1318.48, 1567.96, 1308.1, 1648.1, 1959.95, 1569.72, 1977.72, 2351.94, 1831.34, 2307.34, 2743.93, 2092.96, 2636.96, 3135.92, 2354.58, 2966.58, 3527.91, 2616.2, 3296.2, 3919.9];
	var sig, freq, sum;
	sum = 0;
	freq = EnvGen.kr(Env.new([freqs, freqs, freqs * 2.2, freqs * 2, freqs * 1.3], [5, 5, 5, 5]));
	10.do{
		sig = SinOsc.ar(freq * {Rand(0.99, 1.01)}, {Rand(0.0, 1.0)});
		sum = sum + sig;
	};
	sum = Splay.ar(sum);
	sum = sum * 0.02;
	Out.ar(0, sum);
}).add;
)

~chrd = Synth(\chrd);

///////////////////////////////////////////////////////////////////////////////////

(
SynthDef.new(\glissRand, {
	var freqS, freqA, freq1, freq2, sig, amp, dur;
	dur = Rand(0.3, 0.7);
	freqA = ExpRand(600, 5000);
	freqS = freqA * Rand(0.2, 0.4);
	freq1 = Line.kr(freqA + freqS, freqA, dur * rrand(0.85, 0.95));
	freq2 = Line.kr(freqA - freqS, freqA, dur * rrand(0.85, 0.95));
	amp = EnvGen.kr(Env.perc(0.01, dur), doneAction: 2);
	sig = SinOsc.ar([freq1, freq2], 0.0, amp);
	Out.ar(0, sig * 0.006);
}).add;
)


Synth(\glissRand);

(
~glissTask = Task{
	var synthArray = \glissRand;
	loop{
		Synth(synthArray);
		rrand(0.01, 0.1).wait;
	}
};
)

~glissTask.play;
~glissTask.stop;

//////////////////////////////////

(
SynthDef.new(\raad, {
	arg freqs = #[503.56, 531.11, 559.97, 590.18, 621.80, 654.86, 689.44, 725.57, 763.32, 802.75,
		843.90, 886.84, 931.64, 978.36, 1027.07, 1077.84, 1130.73, 1185.82, 1243.18, 1302.89,
		1365.03, 1429.68, 1496.92, 1566.83, 1639.51, 1715.03, 1793.48, 1874.97, 1959.58, 2047.42,
		2138.58, 2233.17, 2321.30, 2413.08, 2508.62, 2607.94, 2711.17, 2818.43, 2929.84, 3045.53,
		3165.63, 3290.27, 3419.60, 3553.75, 3692.88, 3837.12, 3986.63, 4141.57, 4302.09, 4468.35], ampBus;
	var sig, amp;
	amp = Saw.kr(SinOsc.kr(5).range(1, 10)).range(0.2, 0.8);
	sig = SinOsc.ar(freqs, 0.0, amp);
	sig = Splay.ar(sig);
	sig = BPF.ar(sig, 1750, 5, SinOsc.kr(5).range(1.2, 2.8));
	sig = sig * 0.05 * (1 - In.kr(ampBus));
	Out.ar(0, sig);
}).add;
)

~raad = Synth(\raad);


(
~ampBus = Bus.alloc(\control, s, 1);
SynthDef.new(\safine, {
	arg freqs = #[50.00, 54.08, 58.45, 63.14, 68.16, 73.54, 79.30, 85.46, 92.05, 99.09,
106.60, 114.61, 123.15, 132.24, 141.91, 152.20, 163.14, 174.76, 187.09, 200.17,
214.04, 228.73, 244.28, 260.73, 278.12, 296.50, 315.91, 336.39, 358.00, 380.77,
404.76, 429.99, 456.53, 484.42], ampBus;
	var sig, amp;
	amp = Pulse.kr(SinOsc.kr(2).range(1, 10)).range(0.2, 0.8);
	sig = SinOsc.ar(freqs, 0.0, amp);
	sig = BPF.ar(sig, 450, 5, SinOsc.kr(5).range(1.2, 2.8));
	sig = Splay.ar(sig);
	sig = sig * 0.2 * In.kr(ampBus);
	Out.ar(0, sig);
}).add;
)

~safine = Synth(\safine, [\ampBus, ~ampBus]);
~raad = Synth(\raad, [\ampBus, ~ampBus]);

~ampBus.set(0.1)

(
SynthDef(\lfo, {arg outBus, freq, mode;

	var lfos = Select.kr(mode, [
		LFSaw.kr(freq),
		SinOsc.kr(freq),
		LFDNoise3.kr(freq)
	]);
	Out.kr(outBus, lfos)
}).add
)

a = Synth(\lfo, [\outBus, ~ampBus, \freq, 0.5, \mode, 2])
a.set(\mode, 1, \freq, 6.5)
a.free

//////////////////////////////////////////////////////////////

(
SynthDef.new(\c1, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 32.7], [0.3, 0.3]));     //c1 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

a = Synth(\c1);
a.free

(
SynthDef.new(\c2, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 65.405], [0.3, 0.3]));     //c2 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

b = Synth(\c2);
b.free;

(
SynthDef.new(\c3, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 130.81], [0.3, 0.3]));     //c3 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

c = Synth(\c3);
c.free;

(
SynthDef.new(\c4, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 261.62], [0.3, 0.3]));     //c4 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

d = Synth(\c4);
d.free;

(
SynthDef.new(\c5, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 523.24], [0.3, 0.3]));     //c5 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

f = Synth(\c5);
f.free;

(
SynthDef.new(\c6, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 1046.48], [0.3, 0.5]));     //c6 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

g = Synth(\c6);
g.free;

(
SynthDef.new(\c7, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 2092.96], [0.3, 0.7]));     //c7 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

h = Synth(\c7);
h.free;

(
SynthDef.new(\c8, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 4185.92], [0.3, 1]));     //c8 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

i = Synth(\c8);
i.free;

(
SynthDef.new(\c9, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 8371.84], [0.3, 2]));     //c9 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

j = Synth(\c9);
j.free;


////////////////////////////////////////////////////////////////////////////////////////

(
SynthDef.new(\e2, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 82.405], [0.3, 0.3]));     //e2 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

a = Synth(\e2);
a.free;

(
SynthDef.new(\e3, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([650, 350, 164.81], [0.3, 0.3]));     //e3 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

b = Synth(\e3);
b.free;

(
SynthDef.new(\e4, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([1000, 500, 329.62], [0.3, 0.3]));     //e4 +
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

c = Synth(\e4);
c.free

(
SynthDef.new(\e5, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([800, 500, 659.24], [0.3, 0.3]));     //e5
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

d = Synth(\e5);
d.free

(
SynthDef.new(\e6, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 300, 1318.48], [0.3, 0.3]));     //e6
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

e = Synth(\e6);
e.free

(
SynthDef.new(\e7, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 2636.96], [0.3, 0.3]));     //e7
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

f = Synth(\e7);
f.free

////////////////////////////////////////////////////////////////////////////

(
SynthDef.new(\g2, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 97.9975], [0.3, 0.3]));     //g2
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

a = Synth(\g2);
a.free;

(
SynthDef.new(\g3, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([650, 350, 195.995], [0.3, 0.3]));     //g3
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

b = Synth(\g3);
b.free;

(
SynthDef.new(\g4, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([1000, 500, 391.99], [0.3, 0.3]));     //g4
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

c = Synth(\g4);
c.free

(
SynthDef.new(\g5, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([800, 500, 783.98], [0.3, 0.3]));     //g5
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

d = Synth(\g5);
d.free

(
SynthDef.new(\g6, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 300, 1567.96], [0.3, 0.3]));     //g6
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

e = Synth(\g6);
e.free

(
SynthDef.new(\g7, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 300, 3135.92], [0.3, 0.3]));     //g7
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

f = Synth(\g7);
f.free

///////////////////////////////////////////////////////////////////////////////////////////////

(
SynthDef.new(\b4, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([600, 400, 493.88], [0.3, 0.3]));     //b4
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

a = Synth(\b4);
a.free;

(
SynthDef.new(\b5, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([650, 350, 987.76], [0.3, 0.3]));     //b5
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

b = Synth(\b5);
b.free;

(
SynthDef.new(\b6, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([1000, 500, 1975.52], [0.3, 0.3]));     //b6
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

c = Synth(\b6);
c.free

(
SynthDef.new(\b7, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([800, 500, 3951.04], [0.3, 0.3]));     //b7
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

d = Synth(\b7);
d.free

////////////////////////////////////////////////////////////////////////////

(
SynthDef.new(\d5, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([650, 350, 587.32], [0.3, 0.3]));     //d5
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

b = Synth(\d5);
b.free;

(
SynthDef.new(\d6, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([1000, 500, 1174.64], [0.3, 0.3]));     //d6
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

c = Synth(\d6);
c.free

(
SynthDef.new(\d7, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([800, 500, 2349.28], [0.3, 0.3]));     //d7
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

d = Synth(\d7);
d.free

(
SynthDef.new(\d8, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([800, 500, 4698.56], [0.3, 0.3]));     //d8
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

d = Synth(\d8);
d.free

//////////////////////////////////////////////////////////////////////////////////////

(
SynthDef.new(\a5, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([650, 350, 880], [0.3, 0.3]));     //a5
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

b = Synth(\a5);
b.free;

(
SynthDef.new(\a6, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([1000, 500, 1760], [0.3, 0.3]));     //a6
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

c = Synth(\a6);
c.free

(
SynthDef.new(\a7, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([800, 500, 3520], [0.3, 0.3]));     //a7
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

d = Synth(\a7);
d.free

(
SynthDef.new(\a8, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([800, 500, 7040], [0.3, 0.3]));     //a8
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

d = Synth(\a8);
d.free

//////////////////////////////////////////////////////////////////////////////

(
SynthDef.new(\gis, {
	var sig, freq;
	freq = EnvGen.kr(Env.new([800, 500, 7458.56], [0.3, 0.3]));     //
	sig = SinOsc.ar(freq);
	Out.ar(0, sig * 0.5);
}).add;
)

d = Synth(\gis);
d.free

1864.64
3729.28
7458.56

3729.28 * 2

(60 + 22).midicps;

/////////////////////////////////////////////////////////////////////////////////////

(
Array.fill(10, {
	arg i;
	10 * (log(i * (i * i)));
});
)
[20.79, 32.95, 41.58, 48.28, 53.75, 58.37, 62.38, 65.91] * 2

(
SynthDef.new(\bassLow, {
	arg freqs = #[20.79, 32.95, 41.58, 48.28, 53.75, 58.37, 62.38, 65.91, 83.16, 96.56, 107.5, 116.74, 124.76, 131.82];
	var sig, env, amp;
	amp = LFPulse.kr(5).range(0.5, 0.9);
	env = Line.kr(0, 1, 0.05);
	sig = SinOsc.ar(freqs);
	sig = Splay.ar(sig);
	sig = sig * env /** amp*/;
	Out.ar(0, sig * 0.5);
}).add;
)

d = Synth(\bassLow);
d.free
