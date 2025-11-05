Prg1:

 

import java.util.*;
public class CeasarCipher {
    public static String enc(String txt,int k){
        StringBuilder res = new StringBuilder();
        for (int i = 0; i < txt.length(); i++) {
            char ch = txt.charAt(i);
            if(Character.isUpperCase(ch)){
                char c = (char)((ch + k -'A') %26 +'A');
                res.append(c);
            }
            else if(Character.isLowerCase(ch)){
                char c = (char)((ch + k - 'a')%26 + 'a');
                res.append(c);
            }
            else{
                res.append(ch);
            }
        }
        return res.toString();
    }
================================================================================================================================================
Prg2:
 

import java.util.*;

public class HillCipher {
    static int[][] key = {{3,3},{2,5}};
    static int[][] invKey = {{15,17},{20,9}};

    public static String enc(String txt){
        txt = txt.toUpperCase().replaceAll("[^A-Z]", "");
        if(txt.length()%2!=0)
            txt += 'X';
        StringBuilder cipher = new StringBuilder();
       
        for(int i = 0; i<txt.length(); i= i+2){
            int x1 = txt.charAt(i) - 'A';
            int x2 = txt.charAt(i+1) - 'A';
            int y1 = (key[0][0]*x1 + key[0][1]*x2)% 26;
            int y2 = (key[1][0]*x1 + key[1][1]*x2)% 26;
            cipher.append((char)(y1+'A')).append((char)(y2+'A'));
        }
        return cipher.toString();
    }
    public static String dec(String txt){     
        StringBuilder plain = new StringBuilder();       
        for(int i = 0; i<txt.length(); i= i+2){
            int x1 = txt.charAt(i) - 'A';
            int x2 = txt.charAt(i+1) - 'A';
            int y1 = (invKey[0][0]*x1 + invKey[0][1]*x2)% 26;
            int y2 = (invKey[1][0]*x1 + invKey[1][1]*x2)% 26;
            plain.append((char)(y1+'A')).append((char)(y2+'A'));
        }
        return plain.toString();
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter msg:");
        String msg = sc.nextLine();
        String enc = enc(msg);
        System.out.println("Encrypted:" + enc);
        System.out.println("Decrypted:" + dec(enc));
    }
}
================================================================================================================================================
Prg4:
 

import java.util.*;

public class VigenereCipher {

    public static String enc(String txt, String k){
        StringBuilder res = new StringBuilder();
        k = k.toUpperCase();
        int n = k.length();
        for(int i = 0; i<txt.length(); i++){
            char c = txt.charAt(i);
            if(Character.isLetter(c)){
                char base = Character.isUpperCase(c)?'A':'a';
                int shift = k.charAt(i%n)-'A';
                res.append((char)((c-base+shift)%26+base));
            }
            else{
                res.append(c);
            }
        }
        return res.toString();
    }

    public static String dec(String txt, String k){
        StringBuilder res = new StringBuilder();
        k = k.toUpperCase();
        int n = k.length();
        for(int i = 0; i<txt.length(); i++){
            char c = txt.charAt(i);
            if(Character.isLetter(c)){
                char base = Character.isUpperCase(c)?'A':'a';
                int shift = k.charAt(i%n)-'A';
                res.append((char)((c-base-shift+26)%26+base));
            }
            else{
                res.append(c);
            }
        }
        return res.toString();
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter input:");
        String txt = sc.nextLine();
        System.out.println("Enter Key:");
        String k = sc.nextLine();
        String enc = enc(txt, k);
        System.out.println("Encrypted: " + enc);
        System.out.println("Decrypted: " + dec(enc, k));
        sc.close();
    }
}
===============================================================================================================================================
Prg5:
 

import java.util.*;

public class RailFenceCipher {

    public static String enc(String txt, int k){
        char[][] rail = new char[k][txt.length()];
        for(char[] row:rail)
            Arrays.fill(row, ' ');
        boolean dirDown = false;
        int row = 0, col = 0;

        for(char c: txt.toCharArray()){
            if(row == 0 || row == k - 1)
                dirDown = !dirDown;
            rail[row][col++] = c;
            row += dirDown?1:-1;    
        }
        StringBuilder res = new StringBuilder();
        for (int i = 0; i < k; i++)
            for (int j = 0; j < txt.length(); j++)
                if(rail[i][j]!=' ')
                    res.append(rail[i][j]);    
        return res.toString();
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("enter msg:");
        String txt = sc.nextLine();
        System.out.println("Enter Key:");
        int k = sc.nextInt();
        System.out.println("Encrypted: " + enc(txt,k));
    }
}
================================================================================================================================================
Prg6:
 

import java.util.*;
import javax.crypto.*;

public class DES {
    public static void main(String[] args) {
        try{
            Scanner sc = new Scanner(System.in);
            System.out.print("Enter text to encrypt: ");
            String plnTxt = sc.nextLine();

            KeyGenerator keyGen = KeyGenerator.getInstance("DES");
            SecretKey secKey = keyGen.generateKey();

            Cipher desCipher = Cipher.getInstance("DES/ECB/PKCS5Padding");
           
            desCipher.init(Cipher.ENCRYPT_MODE,secKey);
            byte[] encBytes = desCipher.doFinal(plnTxt.getBytes());
            String encTxt = Base64.getEncoder().encodeToString(encBytes);

            desCipher.init(Cipher.DECRYPT_MODE, secKey);
            byte[] decBytes = desCipher.doFinal(encBytes);
            String decTxt = new String(decBytes);

            System.out.println("\n----- DES Encryption Demonstration -----");
            System.out.println("Original Text     : " + plnTxt);
            System.out.println("Encrypted Text    : " + encTxt);
            System.out.println("Decrypted Text    : " + decTxt);
            System.out.println("----------------------------------------");

            String encKey = Base64.getEncoder().encodeToString(secKey.getEncoded());
            System.out.println("Secret Key(Base64): " + encKey);
        }
        catch(Exception e){
            e.printStackTrace();
        }
    }
}
===============================================================================================================================================
Prg7:
 

import java.util.*;
import java.math.*;

public class RSA {

    private BigInteger n ,d, e;
    private int bitlen = 1024;

    public RSA() {
        Random r = new Random();
        BigInteger p = BigInteger.probablePrime(bitlen, r);
        BigInteger q = BigInteger.probablePrime(bitlen, r);
        n = p.multiply(q);
        BigInteger phi = p.subtract(BigInteger.ONE).multiply(q.subtract(BigInteger.ONE));
        e = BigInteger.valueOf(65537);
        d = e.modInverse(phi);
    }

    public synchronized String enc(String msg){
        return new BigInteger(msg.getBytes()).modPow(e, n).toString();
    }


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
