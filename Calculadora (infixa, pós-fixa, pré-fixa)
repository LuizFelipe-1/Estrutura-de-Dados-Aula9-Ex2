import java.util.Scanner;
import java.util.Stack;

public class Calculadora {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("Calculadora com Notações Infixa, Pós-fixa e Pré-fixa");
        System.out.println("Escolha o tipo de notação:");
        System.out.println("1 - Infixa (ex: 3 + 4 * 5)");
        System.out.println("2 - Pós-fixa (ex: 3 4 5 * +)");
        System.out.println("3 - Pré-fixa (ex: + 3 * 4 5)");
        System.out.print("Opção: ");
        
        int opcao = scanner.nextInt();
        scanner.nextLine();
        
        System.out.print("Digite a expressão: ");
        String expressao = scanner.nextLine();
        
        double resultado = 0;
        String infixa = "";
        String posfixa = "";
        String prefixa = "";
        
        switch(opcao) {
            case 1:
                infixa = expressao;
                posfixa = infixaParaPosfixa(expressao);
                prefixa = infixaParaPrefixa(expressao);
                resultado = calcularPosfixa(posfixa);
                break;
            case 2:
                posfixa = expressao;
                infixa = posfixaParaInfixa(expressao);
                prefixa = infixaParaPrefixa(infixa);
                resultado = calcularPosfixa(posfixa);
                break;
            case 3:
                prefixa = expressao;
                infixa = prefixaParaInfixa(expressao);
                posfixa = infixaParaPosfixa(infixa);
                resultado = calcularPrefixa(prefixa);
                break;
            default:
                System.out.println("Opção inválida!");
                return;
        }
        
        System.out.println("\nResultado: " + resultado);
        System.out.println("Notação Infixa: " + infixa);
        System.out.println("Notação Pós-fixa: " + posfixa);
        System.out.println("Notação Pré-fixa: " + prefixa);
    }
    
    public static String infixaParaPosfixa(String infixa) {
        String posfixa = "";
        Stack<Character> pilha = new Stack<>();
        
        for (int i = 0; i < infixa.length(); i++) {
            char c = infixa.charAt(i);
            
            if (Character.isDigit(c) || c == '.') {
                posfixa += c;
            } else if (c == ' ') {
                if (!posfixa.isEmpty() && posfixa.charAt(posfixa.length() - 1) != ' ') {
                    posfixa += ' ';
                }
            } else if (c == '(') {
                pilha.push(c);
            } else if (c == ')') {
                while (!pilha.isEmpty() && pilha.peek() != '(') {
                    posfixa += ' ' + pilha.pop().toString();
                }
                pilha.pop(); 
            } else { 
                posfixa += ' ';
                while (!pilha.isEmpty() && precedencia(c) <= precedencia(pilha.peek())) {
                    posfixa += pilha.pop().toString() + ' ';
                }
                pilha.push(c);
            }
        }
        
        while (!pilha.isEmpty()) {
            posfixa += ' ' + pilha.pop().toString();
        }
        
        return posfixa.trim().replaceAll(" +", " ");
    }
    
    public static String infixaParaPrefixa(String infixa) {
        String invertida = new StringBuilder(infixa).reverse().toString();
        invertida = invertida.replace('(', '@').replace(')', '(').replace('@', ')');
        String posfixaInvertida = infixaParaPosfixa(invertida);
        return new StringBuilder(posfixaInvertida).reverse().toString();
    }
    

    public static String posfixaParaInfixa(String posfixa) {
        Stack<String> pilha = new Stack<>();
        String[] tokens = posfixa.split("\\s+");
        
        for (String token : tokens) {
            if (token.matches("[+\\-*/]")) {
                String op2 = pilha.pop();
                String op1 = pilha.pop();
                pilha.push("(" + op1 + " " + token + " " + op2 + ")");
            } else {
                pilha.push(token);
            }
        }
        
        return pilha.pop().replaceAll("\\(|\\)", "");
    }
    
    public static String prefixaParaInfixa(String prefixa) {
        Stack<String> pilha = new Stack<>();
        String[] tokens = prefixa.split("\\s+");
        
        for (int i = tokens.length - 1; i >= 0; i--) {
            String token = tokens[i];
            if (token.matches("[+\\-*/]")) {
                String op1 = pilha.pop();
                String op2 = pilha.pop();
                pilha.push("(" + op1 + " " + token + " " + op2 + ")");
            } else {
                pilha.push(token);
            }
        }
        
        return pilha.pop().replaceAll("\\(|\\)", "");
    }
    
    public static double calcularPosfixa(String posfixa) {
        Stack<Double> pilha = new Stack<>();
        String[] tokens = posfixa.split("\\s+");
        
        for (String token : tokens) {
            if (token.matches("-?\\d+(\\.\\d+)?")) {
                pilha.push(Double.parseDouble(token));
            } else {
                double op2 = pilha.pop();
                double op1 = pilha.pop();
                switch (token) {
                    case "+": pilha.push(op1 + op2); break;
                    case "-": pilha.push(op1 - op2); break;
                    case "*": pilha.push(op1 * op2); break;
                    case "/": pilha.push(op1 / op2); break;
                }
            }
        }
        
        return pilha.pop();
    }
    
    public static double calcularPrefixa(String prefixa) {
        Stack<Double> pilha = new Stack<>();
        String[] tokens = prefixa.split("\\s+");
        
        for (int i = tokens.length - 1; i >= 0; i--) {
            String token = tokens[i];
            if (token.matches("-?\\d+(\\.\\d+)?")) {
                pilha.push(Double.parseDouble(token));
            } else {
                double op1 = pilha.pop();
                double op2 = pilha.pop();
                switch (token) {
                    case "+": pilha.push(op1 + op2); break;
                    case "-": pilha.push(op1 - op2); break;
                    case "*": pilha.push(op1 * op2); break;
                    case "/": pilha.push(op1 / op2); break;
                }
            }
        }
        
        return pilha.pop();
    }
    
    private static int precedencia(char op) {
        switch(op) {
            case '+':
            case '-':
                return 1;
            case '*':
            case '/':
                return 2;
            default:
                return 0;
        }
    }
}
