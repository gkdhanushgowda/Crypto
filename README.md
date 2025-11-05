    public synchronized String dec(String msg){
        return new String(new BigInteger(msg).modPow(d, n).toByteArray());
    }


    public static void main(String[] args) {
        RSA rsa = new RSA();
        String txt = "HELLO";
        String enc = rsa.enc(txt);
        String dec = rsa.dec(enc);
        System.out.println("Original msg: " + txt);
        System.out.println("Enc msg: " + enc);
        System.out.println("Dec msg: " + dec);
    }
}
================================================================================================================================================
Prg8:
 

import java.util.*;
import java.math.*;

public class DiffeHellman {
    public static void main(String args[]){
        BigInteger P = new BigInteger("23");
        BigInteger G = new BigInteger("5");

        BigInteger a = new BigInteger("6");
        BigInteger A = G.modPow(a, P);

        BigInteger b = new BigInteger("15");
        BigInteger B = G.modPow(b, P);

        BigInteger k1 = B.modPow(a, P);
        BigInteger k2 = A.modPow(b, P);

        System.out.println("Alice's key: " + k1);
        System.out.println("Bob's key: " + k2);

    }
}
===========================================================================================================================================
