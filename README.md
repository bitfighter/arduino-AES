Minimal implementation of AES in counter mode with a key length of 256 bit.

Quickstart:
-----------

	#include <aes.h>
	AES aes;
	for (char i = 0; i < 32; i++) {
		aes.key[i] = i; // set the key
	}
	for (char i = 0; i < 15; i++) {
		aes.counter[i] = 0;
	}
	aes.counter[15] = 1;

	char msg[] = "Hello world";
	// encrypt msg in place
	for (char i = 0; i < 11; i++) {
		aes.process(&msg[i]);
	}

The implementation and header file are aes-demo/aes.{cpp,h}.
The code uses approximately 3k of program memory.

Demo Arduino sketch:
--------------------

For a simple demo sketch, plug the arduino into /dev/ttyUSB0 (or change the parameter below), and run these commands:

	cd aes-demo
	make upload
	./aes-demo.py /dev/ttyUSB0

Now you can enter a string. Press enter to send it to the Arduino. The Arduino will encrypt it and send it back. The python script will decrypt it again.

To keep the demo code simple, a restart of the python program requires a restart of the arduino.

For this to work, the arduino-mk package needs to be installed.
Otherwise, the Arduino IDE can be used instead.
The python script requires python3, Crypto, and serial.
This is just for demonstration purposes.
The AES library does of course not require python.

How AES in counter mode works:
------------------------------

This is just a vague description. For more details, see
https://en.wikipedia.org/wiki/Advanced_Encryption_Standard
https://en.wikipedia.org/wiki/Block_cipher_modes_of_operation#Counter_.28CTR.29

AES uses a 32 byte key to encrypt a block of 16 bytes.
For a given key, the same block of plaintext will always be encrypted to the same block of ciphertext.

In counter mode, AES is used to encrypt a 16 byte counter. The result is XOR-ed with a block of plaintext, and the counter is incremented. This basically provides a one-time-pad stream.

The resulting encryption scheme can be used to encrypt 16*(2**(16*8)) bytes.

Note that there is no message authentication or validation. If such a thing is desired it must be provided upstream.
