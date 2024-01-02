# gerenciador-de-estacionamento em C#
gerenciador de estacionamento

using System;
using System.Collections.Generic;

namespace CadastroVeiculos
{
    class Program
    {
        // Dicionário para armazenar os registros dos veículos (declarado como variável global)
        static Dictionary<string, Dictionary<string, string>> registrosVeiculos = new Dictionary<string, Dictionary<string, string>>();

        static void Main()
        {
            Menu();
        }

        public static void Menu()
        {
            while (true)
            {
                Console.WriteLine(@"Seja bem-vindo ao SpacePark onde o seu carro ganha o espaço que merece.
Com preços acessíveis, nós cuidamos do seu carro.
A partir de 16 reais a hora");

                Console.WriteLine("-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-");

                Console.WriteLine(@"Para cadastrar o veículo, digite [1]
Para remover o veículo, digite [2]
Para listar os veículos, digite [3]
Para encerrar o programa, digite [4]: ");

                int opcao = int.Parse(Console.ReadLine());

                Console.WriteLine("-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-");

                switch (opcao)
                {
                    case 1:
                        CadastrarVeiculo();
                        break;
                    case 2:
                        RemoverVeiculo();
                        break;
                    case 3:
                        ListarVeiculos();
                        break;
                    case 4:
                        Console.WriteLine("Encerrando o programa...");
                        return; // Encerra o método Menu e, portanto, o programa
                    default:
                        Console.WriteLine("Opção inválida. Por favor, escolha uma opção válida.");
                        break;
                }
            }
        }

        public static void CadastrarVeiculo()
        {
            Console.WriteLine("Digite a placa do veículo: ");
            string placaVeiculo = Console.ReadLine().ToUpper();

            Console.WriteLine(@"Você digitou os dados do veículo corretamente? 
[Para sim digite S e para Não digite N]: ");
            string confirmar = Console.ReadLine().ToUpper();

            if (confirmar == "S")
            {
                // Obter a data e hora atuais
                DateTime dataHoraAtual = DateTime.Now;

                // Criar um dicionário para armazenar os dados do veículo
                Dictionary<string, string> dadosVeiculo = new Dictionary<string, string>();
                dadosVeiculo.Add("Placa", placaVeiculo);
                dadosVeiculo.Add("DataCadastro", dataHoraAtual.ToString("dd/MM/yyyy HH:mm:ss"));

                // Adicionar o dicionário de dadosVeiculo ao dicionário principal registrosVeiculos
                registrosVeiculos.Add(placaVeiculo, dadosVeiculo);

                Console.WriteLine("Veículo cadastrado com sucesso!");
            }
            else
            {
                Console.WriteLine("Cadastro cancelado.");
            }
        }

        public static void RemoverVeiculo()
        {
            Console.WriteLine(@"Para remover o veículo, digite a placa do veículo: ");
            string placaVeiculo = Console.ReadLine().ToUpper(); // Converta para maiúsculas

            if (registrosVeiculos.ContainsKey(placaVeiculo))
            {
                // Obter a data e hora atuais
                DateTime dataHoraAtual = DateTime.Now;

                // Obter a data e hora de entrada do veículo
                DateTime dataHoraEntrada = DateTime.Parse(registrosVeiculos[placaVeiculo]["DataCadastro"]);

                // Calcular o tempo decorrido
                TimeSpan tempoDecorrido = dataHoraAtual - dataHoraEntrada;

                // Obter o custo por hora (vamos assumir R$ 16 por hora)
                decimal custoPorHora = 16;

                // Calcular o custo total, arredondando para cima
                decimal custoTotal = Math.Ceiling((decimal)tempoDecorrido.TotalHours) * custoPorHora;

                // Exibir informações
                Console.WriteLine("-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-");
                Console.WriteLine($"Tempo decorrido: {tempoDecorrido}");
                Console.WriteLine($"Custo total: R$ {custoTotal}");
                Console.WriteLine("-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-");


                // Remover o veículo do dicionário
                registrosVeiculos.Remove(placaVeiculo);

                Console.WriteLine("Veículo removido com sucesso!");
            }
            else
            {
                Console.WriteLine("Veículo não encontrado. Verifique a placa e tente novamente.");
            }
        }

        public static void ListarVeiculos()
        {
            Console.WriteLine("Lista de Veículos:");

            foreach (var veiculo in registrosVeiculos)
            {
                foreach (var info in veiculo.Value)
                {
                    Console.WriteLine($"{info.Key}: {info.Value}");
                }
                Console.WriteLine("-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-");
            }
        }
    }
}

