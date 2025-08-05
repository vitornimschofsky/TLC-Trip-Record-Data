# Análises solicitadas no case
# A primeira pergunta de análise é: "Qual a média de valor total (total_amount) recebido em um mês considerando todos os yellow táxis da frota?"
print("Média de valor total (total_amount) por mês para yellow táxis:")
spark.sql("""
    SELECT
        f.year,
        f.month,
        round(AVG(f.total_amount),2) AS media_valor_total_mensal
    FROM fact_trips AS f
    JOIN dim_vendor AS v ON f.vendor_id = v.vendor_id
    WHERE v.taxi_type = 'yellow'
    GROUP BY f.year, f.month
    ORDER BY f.year, f.month
""").show()

# A segunda pergunta de análise é: "Qual a média de passageiros (passenger_count) por cada hora do dia que pegaram táxi no mês de maio considerando todos os táxis da frota?"
print("Média de passageiros (passenger_count) por hora em maio para todos os táxis:")
spark.sql("""
    SELECT
        HOUR(f.pickup_datetime) AS hora_do_dia,
        round(AVG(f.passenger_count),2) AS media_passageiros
    FROM fact_trips AS f
    WHERE f.year = 2023 AND f.month = 5
    GROUP BY 1
    ORDER BY 1
""").show(50)
