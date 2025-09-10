# Princípios SOLID: Guia Completo com Exemplos

Os princípios SOLID são um conjunto de diretrizes que visam melhorar a qualidade do design de software na programação orientada a objetos (POO). Criados por Robert C. Martin (também conhecido como Uncle Bob), esses princípios ajudam desenvolvedores a criar sistemas mais flexíveis, compreensíveis e fáceis de manter.

## 📋 Índice
- [Single Responsibility Principle](#single-responsibility-principle)
- [Open/Closed Principle](#openclosed-principle)
- [Liskov Substitution Principle](#liskov-substitution-principle)
- [Interface Segregation Principle](#interface-segregation-principle)
- [Dependency Inversion Principle](#dependency-inversion-principle)
- [Conclusão](#conclusão)

## Single Responsibility Principle

**Uma classe deve ter apenas uma razão para mudar, ou seja, ela deve ter apenas uma única responsabilidade.**

### ❌ Violação do Princípio

```java
class Relatorio {
    public void gerarRelatorio() {
        // Lógica para gerar relatório
    }
    
    public void enviarPorEmail() {
        // Lógica para enviar por e-mail
    }
}
```

### ✅ Solução

```java
class Relatorio {
    public void gerarRelatorio() {
        // Lógica para gerar relatório
    }
}

class EmailService {
    public void enviarPorEmail(Relatorio relatorio) {
        // Lógica para enviar por e-mail
    }
}
```

## Open/Closed Principle

**Software deve ser aberto para extensão, mas fechado para modificação.**

### ❌ Violação do Princípio

```java
class CalculadoraSalario {
    public double calcularSalario(String tipoFuncionario) {
        if (tipoFuncionario.equals("CLT")) {
            // Cálculo para CLT
        } else if (tipoFuncionario.equals("PJ")) {
            // Cálculo para PJ
        }
        // Novos tipos exigem modificação desta classe
    }
}
```

### ✅ Solução

```java
abstract class Funcionario {
    public abstract double calcularSalario();
}

class FuncionarioCLT extends Funcionario {
    @Override
    public double calcularSalario() {
        // Cálculo específico para CLT
    }
}

class FuncionarioPJ extends Funcionario {
    @Override
    public double calcularSalario() {
        // Cálculo específico para PJ
    }
}
```

## Liskov Substitution Principle

**Objetos de uma classe derivada devem poder substituir objetos da classe base sem alterar o comportamento desejado do programa.**

### ❌ Violação do Princípio

```java
class Veiculo {
    public void ligarMotor() {
        // Lógica para ligar motor
    }
}

class Carro extends Veiculo {
    // Faz sentido ter motor
}

class Bicicleta extends Veiculo {
    // NÃO faz sentido ter motor!
}
```

### ✅ Solução

```java
class Veiculo {
    // Funcionalidades comuns a todos os veículos
}

interface VeiculoComMotor {
    void ligarMotor();
}

class Carro extends Veiculo implements VeiculoComMotor {
    @Override
    public void ligarMotor() {
        // Implementação específica
    }
}

class Bicicleta extends Veiculo {
    // Não precisa implementar ligarMotor
}
```

## Interface Segregation Principle

**Os clientes não devem ser forçados a depender de interfaces que não utilizam.**

### ❌ Violação do Princípio

```java
interface Funcionario {
    void trabalhar();
    void fazerCafe();
}

class Programador implements Funcionario {
    public void trabalhar() {
        // Implementação
    }
    
    public void fazerCafe() {
        // Programador não deveria ser obrigado a fazer café!
    }
}
```

### ✅ Solução

```java
interface Funcionario {
    void trabalhar();
}

interface FazCafe {
    void fazerCafe();
}

class Programador implements Funcionario {
    public void trabalhar() {
        // Implementação
    }
}

class Estagiario implements Funcionario, FazCafe {
    public void trabalhar() {
        // Implementação
    }
    
    public void fazerCafe() {
        // Implementação
    }
}
```

## Dependency Inversion Principle

**Dependa de abstrações, não de implementações. Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem dependender de abstrações.**

### ❌ Violação do Princípio

```java
class FileLogger {
    public void log(String message) {
        // Salva log em arquivo
    }
}

class LogService {
    private FileLogger logger = new FileLogger();
    
    public void registrarLog(String message) {
        logger.log(message);
    }
}
```

### ✅ Solução

```java
interface Logger {
    void log(String message);
}

class FileLogger implements Logger {
    @Override
    public void log(String message) {
        // Salva log em arquivo
    }
}

class DatabaseLogger implements Logger {
    @Override
    public void log(String message) {
        // Salva log em banco de dados
    }
}

class LogService {
    private Logger logger;
    
    public LogService(Logger logger) {
        this.logger = logger;
    }
    
    public void registrarLog(String message) {
        logger.log(message);
    }
}
```

## Conclusão

Os princípios SOLID são essenciais para o desenvolvimento de software de alta qualidade. Eles ajudam a criar sistemas mais flexíveis, modulares e de fácil manutenção, o que é fundamental em ambientes de desenvolvimento ágeis e em constante mudança.

| Princípio | Descrição | Benefício |
|-----------|-----------|-----------|
| **S**RP | Uma classe, uma responsabilidade | Código mais coeso e fácil de manter |
| **O**CP | Aberto para extensão, fechado para modificação | Menor risco de introduzir bugs |
| **L**SP | Subclasses substituem superclasses | Hierarquias de classe mais robustas |
| **I**SP | Interfaces específicas ao invés de genéricas | Menos dependências desnecessárias |
| **D**IP | Depender de abstrações, não implementações | Código mais flexível e testável |

Incorporar esses princípios no dia a dia do desenvolvimento pode melhorar significativamente a qualidade do código e a eficiência das equipes.
