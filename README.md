using System;
using System.Collections.Generic;

// Classe que representa um hóspede
public class Pessoa
{
    public string Nome { get; set; }

    public Pessoa(string nome)
    {
        Nome = nome;
    }
}

// Classe que representa uma suíte
public class Suite
{
    public string Nome { get; set; }
    public int Capacidade { get; set; }
    public decimal ValorDiaria { get; set; }

    public Suite(string nome, int capacidade, decimal valorDiaria)
    {
        Nome = nome;
        Capacidade = capacidade;
        ValorDiaria = valorDiaria;
    }
}

// Classe que representa uma reserva
public class Reserva
{
    public List<Pessoa> Hospedes { get; set; } = new List<Pessoa>();
    public Suite Suite { get; set; }
    public int DiasReservados { get; set; }

    public Reserva(int diasReservados)
    {
        DiasReservados = diasReservados;
    }

    public void CadastrarHospedes(List<Pessoa> hospedes)
    {
        if (Suite == null)
        {
            throw new Exception("É necessário atribuir uma suíte antes de cadastrar os hóspedes.");
        }

        if (hospedes.Count > Suite.Capacidade)
        {
            throw new Exception("Número de hóspedes excede a capacidade da suíte.");
        }

        Hospedes = hospedes;
    }

    public int ObterQuantidadeHospedes()
    {
        return Hospedes.Count;
    }

    public decimal CalcularValorDiaria()
    {
        decimal valorTotal = DiasReservados * Suite.ValorDiaria;

        if (DiasReservados >= 10)
        {
            valorTotal *= 0.9m; // Aplica desconto de 10%
        }

        return valorTotal;
    }
}

// Programa principal para testar a implementação
class Program
{
    static void Main(string[] args)
    {
        // Criando hóspedes
        Pessoa hospede1 = new Pessoa("Mauricio");
        Pessoa hospede2 = new Pessoa("Ana");

        List<Pessoa> hospedes = new List<Pessoa> { hospede1, hospede2 };

        // Criando suíte
        Suite suite = new Suite("Suíte Luxo", 2, 250m);

        // Criando reserva
        Reserva reserva = new Reserva(12); // 12 dias -> desconto de 10%
        reserva.Suite = suite;

        try
        {
            reserva.CadastrarHospedes(hospedes);
            Console.WriteLine($"Quantidade de hóspedes: {reserva.ObterQuantidadeHospedes()}");
            Console.WriteLine($"Valor total da diária: R$ {reserva.CalcularValorDiaria():F2}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Erro: {ex.Message}");
        }
    }
}
