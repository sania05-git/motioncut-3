import React, { useState, useEffect } from 'react';
import { ExpenseForm } from './components/ExpenseForm';
import { ExpenseList } from './components/ExpenseList';
import { ExpenseSummary } from './components/ExpenseSummary';
import { Expense } from './types';
import { Receipt } from 'lucide-react';

function App() {
  const [expenses, setExpenses] = useState<Expense[]>(() => {
    const saved = localStorage.getItem('expenses');
    return saved ? JSON.parse(saved) : [];
  });

  useEffect(() => {
    localStorage.setItem('expenses', JSON.stringify(expenses));
  }, [expenses]);

  const handleAddExpense = (expense: Expense) => {
    setExpenses(prev => [expense, ...prev]);
  };

  const handleDeleteExpense = (id: string) => {
    setExpenses(prev => prev.filter(expense => expense.id !== id));
  };

  return (
    <div className="min-h-screen bg-gray-100">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <div className="flex items-center justify-center mb-8">
          <Receipt className="h-8 w-8 text-indigo-600" />
          <h1 className="ml-3 text-3xl font-bold text-gray-900">Expense Tracker</h1>
        </div>

        <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
          <div className="lg:col-span-1">
            <ExpenseForm onAddExpense={handleAddExpense} />
          </div>
          
          <div className="lg:col-span-2 space-y-8">
            <ExpenseSummary expenses={expenses} />
            
            <div className="bg-white rounded-lg shadow-md overflow-hidden">
              <div className="px-6 py-4 border-b border-gray-200">
                <h2 className="text-lg font-medium text-gray-900">Recent Expenses</h2>
              </div>
              <ExpenseList 
                expenses={expenses} 
                onDeleteExpense={handleDeleteExpense} 
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

export default App;
