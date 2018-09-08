import java.net.*;
import java.io.*;
import java.util.*;
import java.nio.file.*;
 public class VocabAnalyzer {
     public static void main(String[] args) throws IOException {
 public static void main(String[] args) throws IOException {
        //
        Scanner scnr = new Scanner(System.in);
        System.out.println("Would You Like to Vocab Check a Book? (Y/N)");
        while (processMenu(scnr)) {
            System.out.println("\nWould you like to check another book? (Y/N)");
        }
        //
     }
     public static String readFile(String path) throws IOException {
        Path file = Paths.get(path);
        StringBuilder sb = new StringBuilder();
@@ -28,7 +17,6 @@ public class VocabAnalyzer {
        }
        return sb.toString();
    }
     public static void group(ArrayList<Vocab> list) {
        for (int i = 0; i < list.size() - 2; i++) {
            while (list.get(i).getVocabWord().equalsIgnoreCase(list.get(i + 1).getVocabWord())) {
@@ -37,7 +25,6 @@ public class VocabAnalyzer {
            }
        }
    }
     //
    private static boolean getMenuChoice(Scanner scnr) {
        String response = scnr.next();
@@ -52,12 +39,10 @@ public class VocabAnalyzer {
        }
    }
    //
     //
    private static boolean processMenu(Scanner scnr) {
        if (getMenuChoice(scnr)) {
            File txtFile = getInputFile(scnr);
             //File f = new File("Words3.txt");
            //FileWriter fw = new FileWriter(f);
            //BufferedWriter bw = new BufferedWriter(fw);
@@ -96,23 +81,22 @@ public class VocabAnalyzer {
                }
                if (txtFile.canRead()) {
                    runAnalysis(list);
                    return true;
                }
            } catch (IOException e) {
                runQuiz(list, scnr);
                return true;
            } catch (IOException e){
                System.out.println("Book wasn't found.");
                return true;
            }
        }
        return false;
    }
    //
     //
    //Read and create a file with user path and name
    private static File getInputFile(Scanner scnr) {
        //An array to split the name and path of the file
        String[] pathAndName = (inquireFile(scnr)).split(",");
         //Creating a new file
        File file = new File(pathAndName[0] + pathAndName[1]);
        while (!file.exists()) {
@@ -124,11 +108,9 @@ public class VocabAnalyzer {
        System.out.println("\nBook was found!");
        System.out.println("Can be read: " + file.canRead());
        System.out.println("Length (in bytes): " + file.length() + "\n");
         return file;
    }
    //
     //
    //Take in the name of the file and the path to the file
    //Will return a string back with path and the name of the file
@@ -139,12 +121,34 @@ public class VocabAnalyzer {
        fileName = fileName.trim() + ".txt";
        System.out.println("Please enter the path for the book:");
        String path = scnr.nextLine();
        path = path.trim() + "/";
         path = path.trim() + "\\";
        return path + "," + fileName;
    }
    //
     private static void runQuiz(ArrayList<Vocab> list, Scanner scnr) throws IOException {
        try {
            int rndInt;
            Random rnd = new Random();
            File vocabSheet = new File("vocab.txt");
            PrintWriter writeVocab = new PrintWriter(new FileWriter(vocabSheet, true));
            System.out.println("\nBefore you read this book, here are some vocabulary words that appear. " +
                    "\nLet's see if you know most of them before reading this book.\n ");
            for (int i = 0; i < 15; ++ i) {
                rndInt = rnd.nextInt(list.size() - 1);
                String word = list.get(rndInt).getVocabWord();
                System.out.print("Do you know the word " + word + " ? (Y/N)");
                if (!getMenuChoice(scnr)) {
                    writeVocab.println(word + " -- http://www.dictionary.com/browse/" +
                            word + "?s=t");
                }
            }
            writeVocab.close();
        } catch (FileNotFoundException e) {
            System.out.println("The book was not found for analysis.");
        } catch (UnsupportedEncodingException e) {
            System.out.println("New vocab file cannot be written.");
        }
    }
    //
    private static void runAnalysis(ArrayList<Vocab> list) {
        System.out.println("Running Analysis...");
@@ -153,15 +157,13 @@ public class VocabAnalyzer {
            PrintWriter writeVocab = new PrintWriter("vocab.txt", "UTF-8");
            int wordCount = 0;
            for (int i = 0; i < list.size(); ++i) {
                if (list.get(i).getVocabWord().length() > 6 &&
                        list.get(i).getFrequency() < 2) {
                    writeVocab.println(list.get(i).getVocabWord() +
                            " -- http://www.dictionary.com/browse/" +
                            list.get(i).getVocabWord() + "?s=t");
                    ++wordCount;
                if (!(list.get(i).getVocabWord().length() > 6 &&
                        list.get(i).getFrequency() < 4)) {
                    list.remove(i);
                    i--;
                }
            }
            writeVocab.println("Total: " + wordCount + " words.");
            writeVocab.println(wordCount);
            writeVocab.close();
            System.out.printf("Vocab file is located at %s%n", vocabSheet.getAbsolutePath());
        } catch (FileNotFoundException e) {
@@ -172,11 +174,36 @@ public class VocabAnalyzer {
    }
    //
}
 class LexicographicComparator implements Comparator<Vocab> {
    @Override
    public int compare(Vocab a, Vocab b) {
        return a.vocabWord.compareToIgnoreCase(b.vocabWord);
    }
    
public class Vocab {
    String vocabWord;
    int frequency;
    public Vocab () {
        vocabWord = "";
        frequency = 0;
    }
    public Vocab (String vocabWord, int frequency) {
        this.vocabWord = vocabWord;
        this.frequency = frequency;
    }
    public int getFrequency() {
        return frequency;
    }
    public void setFrequency (int frequency) {
        this.frequency = frequency;
    }
    public String getVocabWord () {
        return vocabWord;
    }
    public void setVocabWord (String vocabWord) {
        this.vocabWord = vocabWord;
    }
    public void frequencyIncrementer () {
        frequency ++;
    }
}
