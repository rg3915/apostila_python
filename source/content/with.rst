with


	É usado para encapsular a execução de um bloco com métodos definidos por um gerenciador de contexto. 


$ cat << EOF > /tmp/numbers.txt
1
2
3
EOF


$ cat << EOF > /tmp/numbers_str.txt
1
2
three
EOF



f = open('/tmp/numbers.txt', 'r')
for line in f:
    print(int(line))
f.close()
print(f.closed)

1
2
3
True


f = open('/tmp/numbers_str.txt', 'r')
for line in f:
    print(int(line))
f.close()
print(f.closed)


1
2

ValueError: invalid literal for int() with base 10: 'three\n'

f.closed
False

f.close()

f.closed
True



try:
    f = open('/tmp/numbers_str.txt', 'r')
    for line in f:
        print(int(line))
except ValueError: 
    print('Ops... Isso não é um número em forma de dígitos...')
finally:
    f.close()
    print(f.closed)

1
2
Ops... Isso não é um número em forma de dígitos...
True


with open('/tmp/numbers.txt', 'r') as f:
    for line in f:
        print(int(line))
print(f.closed)

1
2
3
True


try:
    with open('/tmp/numbers_str.txt', 'r') as f:
        for line in f:
            print(int(line))
except ValueError:
    print('Ops... Isso não é um número em forma de dígitos...')
finally:
    print(f.closed)

1
2
Ops... Isso não é um número em forma de dígitos...
True


-----------------------------------------------------------------




import psycopg2


# Parâmetros de conexão
PGHOST = 'localhost'
PGDB = 'postgres'
PGPORT = 5432
PGUSER = 'postgres'
PGPASS = '123'
APPLICATION_NAME = 'python'

# Máscara da string de conexão
str_con = 'host={} dbname={} port={} user={} password={} application_name={}'

# String de conexão formatada com os dados
str_con = pg_conn.format(
                         PGHOST,
                         PGDB,
                         PGPORT,
                         PGUSER,
                         PGPASS,
                         APPLICATION_NAME
                        )


> str_sql = "SELECT 'Teste...';"


> class PgConnection(object):
    def __init__(self, str_con, str_sql):
        self.str_con = str_con
        self.str_sql = str_sql


    def __enter__(self):
        print('===== __enter__ =====\n')
        self.conn = psycopg2.connect(str_con)
        cursor = self.conn.cursor()
        cursor.execute(str_sql)
        self.data = cursor.fetchone()
        return self.data

    def __exit__(self, type, value, traceback):
        print('\n===== __exit__ =====')
        self.conn.close()
        return 0


> with PgConnection(str_con, str_sql) as x:
    print(x[0])

===== __enter__ =====

Teste...

===== __exit__ =====