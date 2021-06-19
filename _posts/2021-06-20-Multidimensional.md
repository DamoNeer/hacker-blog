---
title: HSCTF 2021 - multidimensional | Rev
published: true
---

## [](#header-2)Multidimensional

```
import java.util.Scanner;

public class Multidimensional {
	
	private char[][] arr;
	private String mrConnolly;
	
	public Multidimensional(String s) {
		arr = new char[6][6];
		for (int i = 0; i < s.length(); i++) {
			arr[i % 6][i / 6] = s.charAt(i);
		}
		mrConnolly = "hey_since_when_was_time_a_dimension?";
	}
	
	public boolean check() {
		String s = "";
		for (char[] row: arr)
			for (char c: row)
				s += c;
		return s.equals(mrConnolly);
	}
	
	public void line() {
		char[][] newArr = new char[arr.length][arr[0].length];
		for (int i = 0; i < arr.length; i++) {
			for (int j = 0; j < arr[0].length; j++) {
				int p = i - 1, q = j - 1, f = 0;
				boolean row = i % 2 == 0;
				boolean col = j % 2 == 0;
				if (row) {
					p = i + 1;
					f++;
				} else
					f--;
				if (col) {
					q = j + 1;
					f++;
				} else
					f--;
				newArr[p][q] = (char) (arr[i][j] + f);
			}
		}
		arr = newArr;
	}
	
	public void plane() {
		int n = arr.length;
		for (int i = 0; i < n / 2; i++) {
			for (int j = 0; j < n / 2; j++) {
				char t = arr[j][n - 1 -i];
				arr[j][n - 1 -i] = arr[n - 1 - i][n - 1 - j];
				arr[n - 1 - i][n - 1 - j] = arr[n - 1 - j][i];
				arr[n - 1 - j][i] = arr[i][j];
				arr[i][j] = t;
			}
		}
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				arr[i][j] += i + n - j;
			}
		}
	}
	
	public void space(int n) {
		arr[(35 - n) / 6][(35 - n) % 6] -= (n / 6) + (n % 6);
		if (n != 0) {
			n--;
			space(n);
		}
	}
	
	public void time() {
		int[][] t = {{8, 65, -18, -21, -15, 55}, 
				{8, 48, 57, 63, -13, 5}, 
				{16, -5, -26, 54, -7, -2}, 
				{48, 49, 65, 57, 2, 10}, 
				{9, -2, -1, -9, -11, -10}, 
				{56, 53, 18, 42, -28, 5}};
		for (int j = 0; j < arr[0].length; j++)
			for (int i = 0; i < arr.length; i++)
				arr[i][j] += t[j][i];
	}
	
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		System.out.print("Enter the flag: ");
		String s = in.next();
		if (s.length() == 36) {
			Multidimensional f = new Multidimensional(s);
			f.line();
			f.plane();
			f.space(35);
			f.time();
			if (f.check())
				System.out.println("Congratulations! You have transcended beyond dimensions");
			else
				System.out.println("Hmm, that's not correct");
		} else {
			System.out.println("Hmm, that's not correct.");
		}
		in.close();
	}

}

```

### [](#header-3)Solution

The flag string will go through the following functions in that order:

1. Multidimensional()
2. Line()
3. Plane()
4. Space()
5. Time()
6. Check()

Multidimensional() will take the given string and place characters from row by row from left to right in a 6x6 matrix.

Line() will look at the each element's row and column index and see if it's even or odd. It also creates three new variables: "f" for how much the character will be shifted, "p" for the new row index, and "q" for the new column index.

Original Line():

The convention here is (row, col).

even,even will result in f+2 and (i+1,j+1)

even,odd will result in f and (i+1, j)

odd,even will result in f and (i, j+1)

odd,odd will result in f-2 and (i,j)

-------------------------------------------
Reverse of Line():

odd,odd will result in f-2 and (i-1,j-1)

odd, even will result in f and (i-1, j+1)

even, odd will result in f and (i+1, j-1)

even,even will result in f+2 and (i+1, j+1)

Plane() has two steps. It will first go through a bunch reassignments followed by a simple shift. To reverse that, I need to assign them in a reverse order, change the shift direction, and reverse step 2 to become step 1.

```
  public void unplane() {
    int n = arr.length;                //Step 2 of the plane() will execute first
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n; j++) {
        arr[i][j] -= i + n - j;        //It was addition but not reversing it becomes subtraction
      }

    }
    for (int i = 0; i < n / 2; i++) {
      for (int j = 0; j < n / 2; j++) {
        char t = arr[i][j];
        arr[i][j] = arr[n - 1 - j][i];
        arr[n - 1 - j][i] = arr[n - 1 - i][n - 1 - j];
        arr[n - 1 - i][n - 1 - j] = arr[j][n - 1 - i];
        arr[j][n - 1 - i] = t;         //All of these assignment statements have been placed in reverse order from before
      }
    }
    System.out.println("Unplaned: " + Arrays.deepToString(arr)); // after.unplane function
  }
```

Space(35) will look at all 36 elements and shift them by a certain amount to the left. Each element will be shifted differently, but the amount being changed for each element should stay the same during the reverse process because we will be using the same n, which is 35. To reverse space(), I simply need to change the shift order which means subtraction sign to addition sign.

```
  public void unspace(int n) {
    arr[(35 - n) / 6][(35 - n) % 6] += (n / 6) + (n % 6);   //Changed from - to +
    if (n != 0) {
      n--;
      unspace(n);        //Make sure you rename this to your reverse function
    } else {
      System.out.println("Unspaced: " + Arrays.deepToString(arr)); // after.unspace function
    }
  }
```

Time() will shift each element on the matrix by the number that is present in the matrix t. To reverse this, simply change the shift direction by changing the positive sign to negative sign.

```
  public void untime() {
    int[][] t = { { 8, 65, -18, -21, -15, 55 }, { 8, 48, 57, 63, -13, 5 }, { 16, -5, -26, 54, -7, -2 },
        { 48, 49, 65, 57, 2, 10 }, { 9, -2, -1, -9, -11, -10 }, { 56, 53, 18, 42, -28, 5 } };
    for (int j = 0; j < arr[0].length; j++)
      for (int i = 0; i < arr.length; i++)
        arr[i][j] -= t[j][i];          //Changed from + to -
    System.out.println("Untimed: " + Arrays.deepToString(arr)); // after.untime function
  }
```

Check() will see if the results match with the mrcolloy variable in its **matrix form** "hne_anecnt_sye_idi__wmioswaemnihs_e?"

To reverse this function, simply feed this string as the input for the reverse script rather than the original mrcolloy variable.

Here is my reverse script all together: (Please note that I have renamed it to be "demo.java"

```
import java.util.Scanner;
import java.io.*;
import java.util.*;

public class demo {

  private static char[][] arr;
  private String mrConnolly;
  private String to_be_reversed;

  public demo(String s) {
    arr = new char[6][6];
    for (int i = 0; i < s.length(); i++) {
      arr[i % 6][i / 6] = s.charAt(i);
    }
    System.out.println(Arrays.deepToString(arr)); // after.multidimensional function
    mrConnolly = "hey_since_when_was_time_a_dimension?";
    to_be_reversed = "hne_anecnt_sye_idi__wmioswaemnihs_e?";
  }

  public String check() {
    String s = "";
    for (char[] row : arr)
      for (char c : row)
        s += c;
    return (s);
  }

  public void unline() {
    char[][] newArr = new char[arr.length][arr[0].length];
    for (int i = 0; i < arr.length; i++) {
      for (int j = 0; j < arr[0].length; j++) {
        int p = i + 1, q = j + 1, f = 0;
        boolean row = i % 2 != 0;
        boolean col = j % 2 != 0;
        if (row) {
          p = i - 1;
          f--;
        } else
          f++;
        if (col) {
          q = j - 1;
          f--;
        } else
          f++;
        newArr[p][q] = (char) (arr[i][j] + f);
      }
    }
    arr = newArr;
    System.out.println("Unlined: " + Arrays.deepToString(arr)); // after.line function
  }

  public void unplane() {
    int n = arr.length;
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n; j++) {
        arr[i][j] -= i + n - j;
      }

    }
    for (int i = 0; i < n / 2; i++) {
      for (int j = 0; j < n / 2; j++) {
        char t = arr[i][j];
        arr[i][j] = arr[n - 1 - j][i];
        arr[n - 1 - j][i] = arr[n - 1 - i][n - 1 - j];
        arr[n - 1 - i][n - 1 - j] = arr[j][n - 1 - i];
        arr[j][n - 1 - i] = t;
      }
    }
    System.out.println("Unplaned: " + Arrays.deepToString(arr)); // after.unplane function
  }

  public void unspace(int n) {
    arr[(35 - n) / 6][(35 - n) % 6] += (n / 6) + (n % 6);
    if (n != 0) {
      n--;
      unspace(n);
    } else {
      System.out.println("Unspaced: " + Arrays.deepToString(arr)); // after.unspace function
    }
  }

  public void untime() {
    int[][] t = { { 8, 65, -18, -21, -15, 55 }, { 8, 48, 57, 63, -13, 5 }, { 16, -5, -26, 54, -7, -2 },
        { 48, 49, 65, 57, 2, 10 }, { 9, -2, -1, -9, -11, -10 }, { 56, 53, 18, 42, -28, 5 } };
    for (int j = 0; j < arr[0].length; j++)
      for (int i = 0; i < arr.length; i++)
        arr[i][j] -= t[j][i];
    System.out.println("Untimed: " + Arrays.deepToString(arr)); // after.untime function
  }

  public void line() {
    char[][] newArr = new char[arr.length][arr[0].length];
    for (int i = 0; i < arr.length; i++) {
      for (int j = 0; j < arr[0].length; j++) {
        int p = i - 1, q = j - 1, f = 0;
        boolean row = i % 2 == 0;
        boolean col = j % 2 == 0;
        if (row) {
          p = i + 1;
          f++;
        } else
          f--;
        if (col) {
          q = j + 1;
          f++;
        } else
          f--;
        newArr[p][q] = (char) (arr[i][j] + f);
      }
    }
    arr = newArr;
    System.out.println("lined: " + Arrays.deepToString(arr)); // after.line function
  }

  public void plane() {
    int n = arr.length;
    for (int i = 0; i < n / 2; i++) {
      for (int j = 0; j < n / 2; j++) {
        char t = arr[j][n - 1 - i];
        arr[j][n - 1 - i] = arr[n - 1 - i][n - 1 - j];
        arr[n - 1 - i][n - 1 - j] = arr[n - 1 - j][i];
        arr[n - 1 - j][i] = arr[i][j];
        arr[i][j] = t;
      }
    }
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n; j++) {
        arr[i][j] += i + n - j;
      }
    }
    System.out.println("Planed: " + Arrays.deepToString(arr)); // after.unplane function
  }

  public void space(int n) {
    arr[(35 - n) / 6][(35 - n) % 6] -= (n / 6) + (n % 6);
    if (n != 0) {
      n--;
      space(n);
    } else {
      System.out.println("Spaced: " + Arrays.deepToString(arr)); // after.space function
    }
  }

  public void time() {
    int[][] t = { { 8, 65, -18, -21, -15, 55 }, { 8, 48, 57, 63, -13, 5 }, { 16, -5, -26, 54, -7, -2 },
        { 48, 49, 65, 57, 2, 10 }, { 9, -2, -1, -9, -11, -10 }, { 56, 53, 18, 42, -28, 5 } };
    for (int j = 0; j < arr[0].length; j++)
      for (int i = 0; i < arr.length; i++)
        arr[i][j] += t[j][i];
    System.out.println("Timed: " + Arrays.deepToString(arr)); // after.time function
  }

  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    System.out.print("Enter the Reverse flag: ");
    // "hne_anecnt_sye_idi__wmioswaemnihs_e?" from multidimensional(mrcolloy)
    String s = in.next();
    if (s.length() == 36) { // Flag length = 36
      demo f = new demo(s);
      // f.time();
      f.untime();
      // f.space(35);
      f.unspace(35);
      // f.plane();
      f.unplane();
      // f.line();
      f.unline();
      f.check();
      String flag = f.check();
      demo F = new demo(flag);
      System.out.print((F.check()));
      in.close();
    } else {
      System.out.println(s.length());
    }

  }
}
```

![image](https://user-images.githubusercontent.com/81070073/122655020-1954e300-d104-11eb-9af9-21f0c375c819.png)
