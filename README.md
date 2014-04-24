ProjectAssignment1
==================
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ProjectAssignment1
{
    class Program
    {
        class Customer
        {
            private string _Name;
            public string Name { get { return _Name; } }

            private List<SavingsAccount> _Accounts = new List<SavingsAccount>();
            public IReadOnlyCollection<SavingsAccount> Account { get { return _Accounts.AsReadOnly(); } }

            //constructor
            public Customer(string name)
            {
                _Name = name;
            }

            public void AddAccount(SavingsAccount account)
            {
                if (_Accounts.Contains(account) == false)
                {
                    _Accounts.Add(account);
                }

            }

            public override string ToString()
            {
                return string.Format("{0}", _Name);
            }

        }

        class SavingsAccount
        {
            public const decimal DEFAULT_OPENING_BALANCE = 0m;
            private List<Customer> _Owners = new List<Customer>();
            private List<ITransaction> _Transactions = new List<ITransaction>();
            
            private decimal _OpeningBalance;
            public decimal OpeningBalance { get { return _OpeningBalance; } }

            public IReadOnlyCollection<Customer> Owners
            {
                get { return _Owners.AsReadOnly(); }
            }

            public IReadOnlyCollection<ITransaction> Transactions
            {
                get { return _Transactions.AsReadOnly(); }
            }

            //???
            public decimal Balance { get { return _OpeningBalance; } }
            //cont
            public SavingsAccount(decimal OpeningBalance)
            {
                _OpeningBalance = OpeningBalance;
            }
            //???
            public SavingsAccount()
            {
                _OpeningBalance = DEFAULT_OPENING_BALANCE;

            }

            //public interface of ITtransaction
            public interface ITransaction
            {
                string Description { get; }
                decimal Amount { get; }
                decimal Value { get; }

            }


            abstract class DecreaseTransaction : ITransaction
            {
                private string _Description;
                private decimal _Amount;
                private decimal _Value;

                public DecreaseTransaction(string Description, decimal Amount, decimal Value)
                {
                    _Description = Description;
                    _Amount = Amount;
                    _Value = Value;
                }
                public DecreaseTransaction()
                {

                }
                public DecreaseTransaction(decimal Amount)
                {
                    this._Amount = Amount;
                }
                public override string ToString()
                {
                    return string.Format("Description : {0} \n Amount : {1} \n Value : {2}", _Description, _Amount, _Value);
                }
            }


            abstract class IncreaseTransaction : ITransaction
            {
                private string _Description;
                private decimal _Amount;
                private decimal _Value;
                private decimal Amount;

                public IncreaseTransaction(string Description, decimal Amount, decimal Value)
                {
                    _Description = Description;
                    _Amount = Amount;
                    _Value = Value;
                }
                public IncreaseTransaction()
                {

                }
                public IncreaseTransaction(decimal Amount)
                {
                    this._Amount = Amount;
                }

                public override string ToString()
                {
                    return string.Format("Description : {0} \n Amount : {1} \n Value : {2}", _Description, _Amount, _Value);
                }
            }


            class Deposit : IncreaseTransaction
            {
                private decimal _Amount;

                public decimal Amount { get { return _Amount; } }

                public Deposit(decimal Amount) : base(Amount)
                {
                    _Amount = Amount;
                }
                public Deposit(Deposit D)
                {
                    _Amount = D.Amount;
                }

                public override string ToString()
                {
                    return string.Format("Deposit : {0}", _Amount);
                }
            }

            class Withdrawal : DecreaseTransaction
            {
                private decimal _Amount;

                public decimal Amount
                {
                    get { return _Amount; }
                }

                public Withdrawal(decimal Amount) : base(Amount)
                {
                    _Amount = Amount;
                }
                public Withdrawal(Withdrawal W)
                {
                    _Amount = W.Amount;
                }

                public override string ToString()
                {
                    return string.Format("Withdrawal : {0}", _Amount);
                }
            }

            public void DumpCustomers(string textWriter)
            {
                foreach (Customer c in _Customer)
                {
                    textWriter.WriteLine("\t{0}", c);
                    foreach (SavingsAccount a in c.Accounts)
                    {
                        textWriter.WriteLine("\t\t{0}", a.AccountType);
                        textWriter.WriteLine("\t\t\t{0,-20} : {1,13}", "Opening Balance", a.OpeningBalanceString);
                        foreach (ITransaction t in a.Transactions)
                            textWriter.WriteLine("\t\t\t{0,-20} : {1,13}", t.Description, t);
                        textWriter.WriteLine("\t\t\t{0,-20} : {1,13}", "Closing Balance", a);
                    }
                }
            }

            public void DumpAccounts(TextWriter textWriter)
            {
                foreach (SavingsAccount a in _Accounts)
                {
                    textWriter.WriteLine("\t{0,-20}: {1,13}", a.AccountType, a);
                    foreach (Customer c in a.Owners)
                        textWriter.WriteLine("\t\t{0}", c);
                }
            }


            class MainClass
            {
                public static void Main(string[] args)
                {
                    
                }
            }


        }
    }
}
