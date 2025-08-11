import React, { useState, useMemo } from 'react';
import { Plus, Clock, User, FileText, Calendar, Check, Search, Edit2, Trash2, DollarSign, MapPin, Phone, Mail, AlertCircle } from 'lucide-react';

const PaymentSystem = () => {
  const [workers, setWorkers] = useState([
    { 
      id: 1, 
      name: 'Maria Rodriguez', 
      phone: '555-0123', 
      email: 'maria@email.com', 
      defaultRate: 25,
      status: 'Active',
      skills: ['Cleaning', 'Organization']
    },
    { 
      id: 2, 
      name: 'James Wilson', 
      phone: '555-0124', 
      email: 'james@email.com', 
      defaultRate: 20,
      status: 'Active',
      skills: ['Delivery', 'Maintenance']
    },
  ]);

  const [transactions, setTransactions] = useState([
    {
      id: 1,
      workerId: 1,
      workerName: 'Maria Rodriguez',
      service: 'Apartment Cleaning',
      date: '2025-08-10',
      startTime: '09:00',
      endTime: '12:00',
      hours: 3,
      rate: 25,
      total: 75,
      status: 'Paid',
      paymentMethod: 'Cash',
      notes: 'Deep clean of kitchen and bathrooms',
      createdAt: '2025-08-10T09:00:00'
    }
  ]);

  const [serviceTemplates] = useState([
    { name: 'Apartment Cleaning', defaultRate: 25, estimatedHours: 3 },
    { name: 'Deep Cleaning', defaultRate: 30, estimatedHours: 4 },
    { name: 'Delivery Service', defaultRate: 20, estimatedHours: 1 },
    { name: 'Administrative Work', defaultRate: 22, estimatedHours: 2 },
    { name: 'Maintenance', defaultRate: 28, estimatedHours: 2 },
    { name: 'Organization', defaultRate: 24, estimatedHours: 3 },
  ]);

  // Form states
  const [newWorker, setNewWorker] = useState({ 
    name: '', phone: '', email: '', defaultRate: 0, skills: [] 
  });
  const [newTransaction, setNewTransaction] = useState({
    workerId: '',
    service: '',
    date: new Date().toISOString().split('T')[0],
    startTime: '',
    endTime: '',
    hours: 0,
    rate: 0,
    notes: '',
    paymentMethod: 'Cash'
  });

  // UI states
  const [activeTab, setActiveTab] = useState('home');
  const [showNewWorkerForm, setShowNewWorkerForm] = useState(false);
  const [showNewTransactionForm, setShowNewTransactionForm] = useState(false);
  const [editingWorker, setEditingWorker] = useState(null);
  const [searchTerm, setSearchTerm] = useState('');
  const [errors, setErrors] = useState({});
  const [notification, setNotification] = useState(null);

  const paymentMethods = ['Cash', 'Venmo', 'PayPal', 'Zelle', 'Check'];

  // Computed values
  const pendingTransactions = useMemo(() => 
    transactions.filter(t => t.status === 'Pending'), 
    [transactions]
  );
  
  const totalPending = useMemo(() => 
    pendingTransactions.reduce((sum, t) => sum + t.total, 0), 
    [pendingTransactions]
  );

  const recentTransactions = useMemo(() => 
    transactions.slice(-5).reverse(), 
    [transactions]
  );

  // Utility functions
  const showNotification = (message, type = 'success') => {
    setNotification({ message, type });
    setTimeout(() => setNotification(null), 3000);
  };

  const validateWorkerForm = () => {
    const newErrors = {};
    if (!newWorker.name.trim()) newErrors.name = 'Name is required';
    if (!newWorker.phone.trim()) newErrors.phone = 'Phone is required';
    if (!newWorker.email.trim()) newErrors.email = 'Email is required';
    if (newWorker.defaultRate <= 0) newErrors.defaultRate = 'Rate must be greater than 0';
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const validateTransactionForm = () => {
    const newErrors = {};
    if (!newTransaction.workerId) newErrors.workerId = 'Worker is required';
    if (!newTransaction.service) newErrors.service = 'Service is required';
    if (!newTransaction.date) newErrors.date = 'Date is required';
    if (newTransaction.hours <= 0) newErrors.hours = 'Hours must be greater than 0';
    if (newTransaction.rate <= 0) newErrors.rate = 'Rate must be greater than 0';
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const calculateHoursFromTime = (start, end) => {
    if (!start || !end) return 0;
    const startTime = new Date(`2000-01-01T${start}`);
    const endTime = new Date(`2000-01-01T${end}`);
    const diffMs = endTime - startTime;
    return Math.max(0, diffMs / (1000 * 60 * 60));
  };

  // CRUD operations
  const addWorker = () => {
    if (!validateWorkerForm()) return;
    
    const worker = {
      id: Date.now(),
      ...newWorker,
      status: 'Active'
    };
    
    setWorkers([...workers, worker]);
    setNewWorker({ name: '', phone: '', email: '', defaultRate: 0, skills: [] });
    setShowNewWorkerForm(false);
    setErrors({});
    showNotification('Worker added successfully!');
  };

  const updateWorker = () => {
    if (!validateWorkerForm()) return;
    
    setWorkers(workers.map(w => 
      w.id === editingWorker.id ? { ...editingWorker, ...newWorker } : w
    ));
    setEditingWorker(null);
    setNewWorker({ name: '', phone: '', email: '', defaultRate: 0, skills: [] });
    setErrors({});
    showNotification('Worker updated successfully!');
  };

  const deleteWorker = (workerId) => {
    if (window.confirm('Are you sure you want to remove this worker?')) {
      setWorkers(workers.filter(w => w.id !== workerId));
      showNotification('Worker removed successfully!');
    }
  };

  const addTransaction = () => {
    if (!validateTransactionForm()) return;
    
    const worker = workers.find(w => w.id === parseInt(newTransaction.workerId));
    const total = newTransaction.hours * newTransaction.rate;
    
    const transaction = {
      id: Date.now(),
      ...newTransaction,
      workerId: parseInt(newTransaction.workerId),
      workerName: worker.name,
      total: total,
      status: 'Pending',
      createdAt: new Date().toISOString()
    };
    
    setTransactions([...transactions, transaction]);
    
    setNewTransaction({
      workerId: '',
      service: '',
      date: new Date().toISOString().split('T')[0],
      startTime: '',
      endTime: '',
      hours: 0,
      rate: 0,
      notes: '',
      paymentMethod: 'Cash'
    });
    setShowNewTransactionForm(false);
    setErrors({});
    showNotification('Work session recorded successfully!');
  };

  const markAsPaid = (transactionId) => {
    const transaction = transactions.find(t => t.id === transactionId);
    setTransactions(transactions.map(t => 
      t.id === transactionId ? { ...t, status: 'Paid', paidAt: new Date().toISOString() } : t
    ));
    showNotification(`$${transaction.total} payment completed!`);
  };

  const deleteTransaction = (transactionId) => {
    if (window.confirm('Are you sure you want to delete this transaction?')) {
      setTransactions(transactions.filter(t => t.id !== transactionId));
      showNotification('Transaction deleted');
    }
  };

  // Event handlers
  const handleWorkerSelect = (workerId) => {
    const worker = workers.find(w => w.id === parseInt(workerId));
    if (worker) {
      setNewTransaction({
        ...newTransaction,
        workerId: workerId,
        rate: worker.defaultRate
      });
    }
  };

  const handleServiceSelect = (serviceName) => {
    const template = serviceTemplates.find(s => s.name === serviceName);
    if (template) {
      setNewTransaction({
        ...newTransaction,
        service: serviceName,
        rate: template.defaultRate,
        hours: template.estimatedHours
      });
    }
  };

  const handleTimeChange = (field, value) => {
    const updated = { ...newTransaction, [field]: value };
    if (updated.startTime && updated.endTime) {
      updated.hours = calculateHoursFromTime(updated.startTime, updated.endTime);
    }
    setNewTransaction(updated);
  };

  // Filtered data
  const filteredTransactions = useMemo(() => {
    return transactions.filter(t => 
      t.workerName.toLowerCase().includes(searchTerm.toLowerCase()) ||
      t.service.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [transactions, searchTerm]);

  const filteredWorkers = useMemo(() => {
    return workers.filter(w => 
      w.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [workers, searchTerm]);

  return (
    <div className="min-h-screen bg-slate-50">
      {/* Notification */}
      {notification && (
        <div className={`fixed top-6 right-6 z-50 px-6 py-4 rounded-xl shadow-lg flex items-center gap-3 ${
          notification.type === 'success' 
            ? 'bg-emerald-500 text-white' 
            : 'bg-red-500 text-white'
        }`}>
          <Check className="w-5 h-5" />
          <span className="font-medium">{notification.message}</span>
        </div>
      )}

      {/* Header */}
      <div className="bg-white border-b border-slate-200">
        <div className="max-w-6xl mx-auto px-6 py-8">
          <div className="flex items-center justify-between">
            <div className="flex items-center gap-4">
              <div className="w-12 h-12 bg-gradient-to-br from-blue-600 to-indigo-700 rounded-2xl flex items-center justify-center">
                <DollarSign className="w-6 h-6 text-white" />
              </div>
              <div>
                <h1 className="text-2xl font-bold text-slate-900">PayTracker</h1>
                <p className="text-slate-600">Simple payment tracking for your business</p>
              </div>
            </div>
            
            {pendingTransactions.length > 0 && (
              <div className="text-right">
                <div className="inline-flex items-center gap-2 px-4 py-2 bg-amber-50 border border-amber-200 rounded-xl">
                  <AlertCircle className="w-4 h-4 text-amber-600" />
                  <span className="text-sm font-medium text-amber-800">
                    ${totalPending} pending payment{pendingTransactions.length !== 1 ? 's' : ''}
                  </span>
                </div>
              </div>
            )}
          </div>
        </div>
      </div>

      {/* Navigation */}
      <div className="max-w-6xl mx-auto px-6 py-6">
        <div className="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
          <div className="flex">
            {[
              { key: 'home', label: 'Home', icon: MapPin },
              { key: 'transactions', label: 'All Transactions', icon: FileText },
              { key: 'workers', label: 'Workers', icon: User },
              { key: 'reports', label: 'Reports', icon: Calendar }
            ].map(({ key, label, icon: Icon }) => (
              <button
                key={key}
                onClick={() => setActiveTab(key)}
                className={`flex-1 px-6 py-4 text-sm font-medium flex items-center justify-center gap-2 transition-all ${
                  activeTab === key
                    ? 'bg-blue-600 text-white'
                    : 'text-slate-600 hover:text-slate-900 hover:bg-slate-50'
                }`}
              >
                <Icon className="w-4 h-4" />
                {label}
              </button>
            ))}
          </div>
        </div>

        {/* Home Tab */}
        {activeTab === 'home' && (
          <div className="mt-8 space-y-8">
            {/* Quick Actions */}
            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
              <button
                onClick={() => setShowNewTransactionForm(true)}
                className="group bg-white p-8 rounded-2xl shadow-sm border border-slate-200 hover:border-blue-300 hover:shadow-md transition-all text-left"
              >
                <div className="flex items-center gap-4">
                  <div className="w-12 h-12 bg-blue-100 rounded-xl flex items-center justify-center group-hover:bg-blue-200 transition-colors">
                    <Plus className="w-6 h-6 text-blue-600" />
                  </div>
                  <div>
                    <h3 className="text-lg font-semibold text-slate-900">Record Work Session</h3>
                    <p className="text-slate-600">Log completed work and calculate payment</p>
                  </div>
                </div>
              </button>

              <button
                onClick={() => setShowNewWorkerForm(true)}
                className="group bg-white p-8 rounded-2xl shadow-sm border border-slate-200 hover:border-emerald-300 hover:shadow-md transition-all text-left"
              >
                <div className="flex items-center gap-4">
                  <div className="w-12 h-12 bg-emerald-100 rounded-xl flex items-center justify-center group-hover:bg-emerald-200 transition-colors">
                    <User className="w-6 h-6 text-emerald-600" />
                  </div>
                  <div>
                    <h3 className="text-lg font-semibold text-slate-900">Add Worker</h3>
                    <p className="text-slate-600">Add someone new to your team</p>
                  </div>
                </div>
              </button>
            </div>

            {/* Pending Payments */}
            {pendingTransactions.length > 0 && (
              <div className="bg-white rounded-2xl shadow-sm border border-slate-200">
                <div className="px-8 py-6 border-b border-slate-200">
                  <h3 className="text-lg font-semibold text-slate-900">Pending Payments</h3>
                  <p className="text-slate-600">Work sessions that need to be paid</p>
                </div>
                <div className="p-8">
                  <div className="space-y-4">
                    {pendingTransactions.map((transaction) => (
                      <div key={transaction.id} className="flex items-center justify-between p-6 bg-amber-50 border border-amber-200 rounded-xl">
                        <div className="flex items-center gap-4">
                          <div className="w-10 h-10 bg-amber-100 rounded-xl flex items-center justify-center">
                            <Clock className="w-5 h-5 text-amber-600" />
                          </div>
                          <div>
                            <p className="font-semibold text-slate-900">{transaction.workerName}</p>
                            <p className="text-slate-600">{transaction.service} • {transaction.date}</p>
                            <p className="text-sm text-slate-500">{transaction.hours}h at ${transaction.rate}/hr</p>
                          </div>
                        </div>
                        <div className="flex items-center gap-4">
                          <div className="text-right">
                            <p className="text-2xl font-bold text-slate-900">${transaction.total}</p>
                            <p className="text-sm text-slate-500">{transaction.paymentMethod}</p>
                          </div>
                          <button
                            onClick={() => markAsPaid(transaction.id)}
                            className="px-4 py-2 bg-emerald-600 text-white rounded-lg hover:bg-emerald-700 transition-colors font-medium"
                          >
                            Mark Paid
                          </button>
                        </div>
                      </div>
                    ))}
                  </div>
                </div>
              </div>
            )}

            {/* Recent Activity */}
            <div className="bg-white rounded-2xl shadow-sm border border-slate-200">
              <div className="px-8 py-6 border-b border-slate-200">
                <h3 className="text-lg font-semibold text-slate-900">Recent Activity</h3>
                <p className="text-slate-600">Latest work sessions and payments</p>
              </div>
              <div className="p-8">
                {recentTransactions.length > 0 ? (
                  <div className="space-y-4">
                    {recentTransactions.map((transaction) => (
                      <div key={transaction.id} className="flex items-center justify-between p-6 bg-slate-50 rounded-xl">
                        <div className="flex items-center gap-4">
                          <div className={`w-10 h-10 rounded-xl flex items-center justify-center ${
                            transaction.status === 'Paid' 
                              ? 'bg-emerald-100' 
                              : 'bg-amber-100'
                          }`}>
                            {transaction.status === 'Paid' ? (
                              <Check className="w-5 h-5 text-emerald-600" />
                            ) : (
                              <Clock className="w-5 h-5 text-amber-600" />
                            )}
                          </div>
                          <div>
                            <p className="font-semibold text-slate-900">{transaction.workerName}</p>
                            <p className="text-slate-600">{transaction.service}</p>
                            <p className="text-sm text-slate-500">{transaction.date}</p>
                          </div>
                        </div>
                        <div className="text-right">
                          <p className="text-xl font-bold text-slate-900">${transaction.total}</p>
                          <span className={`inline-flex items-center px-3 py-1 rounded-full text-sm font-medium ${
                            transaction.status === 'Paid' 
                              ? 'bg-emerald-100 text-emerald-800' 
                              : 'bg-amber-100 text-amber-800'
                          }`}>
                            {transaction.status}
                          </span>
                        </div>
                      </div>
                    ))}
                  </div>
                ) : (
                  <div className="text-center py-12">
                    <FileText className="w-12 h-12 text-slate-300 mx-auto mb-4" />
                    <p className="text-slate-500">No transactions yet. Record your first work session!</p>
                  </div>
                )}
              </div>
            </div>
          </div>
        )}

        {/* Transactions Tab */}
        {activeTab === 'transactions' && (
          <div className="mt-8 space-y-6">
            <div className="bg-white rounded-2xl shadow-sm border border-slate-200">
              <div className="px-8 py-6 border-b border-slate-200">
                <div className="flex items-center justify-between">
                  <div>
                    <h2 className="text-xl font-bold text-slate-900">All Transactions</h2>
                    <p className="text-slate-600">Complete history of work sessions and payments</p>
                  </div>
                  <button
                    onClick={() => setShowNewTransactionForm(true)}
                    className="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors font-medium flex items-center gap-2"
                  >
                    <Plus className="w-4 h-4" />
                    New Transaction
                  </button>
                </div>
                
                <div className="mt-6">
                  <div className="relative">
                    <Search className="absolute left-4 top-1/2 transform -translate-y-1/2 text-slate-400 w-5 h-5" />
                    <input
                      type="text"
                      placeholder="Search transactions..."
                      value={searchTerm}
                      onChange={(e) => setSearchTerm(e.target.value)}
                      className="w-full pl-12 pr-4 py-3 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                    />
                  </div>
                </div>
              </div>

              <div className="overflow-x-auto">
                <table className="w-full">
                  <thead className="bg-slate-50">
                    <tr>
                      <th className="px-8 py-4 text-left text-sm font-medium text-slate-700">Worker & Service</th>
                      <th className="px-8 py-4 text-left text-sm font-medium text-slate-700">Date & Time</th>
                      <th className="px-8 py-4 text-left text-sm font-medium text-slate-700">Duration & Rate</th>
                      <th className="px-8 py-4 text-left text-sm font-medium text-slate-700">Total</th>
                      <th className="px-8 py-4 text-left text-sm font-medium text-slate-700">Status</th>
                      <th className="px-8 py-4 text-left text-sm font-medium text-slate-700">Actions</th>
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-slate-200">
                    {filteredTransactions.map((transaction) => (
                      <tr key={transaction.id} className="hover:bg-slate-50">
                        <td className="px-8 py-6">
                          <div>
                            <p className="font-semibold text-slate-900">{transaction.workerName}</p>
                            <p className="text-slate-600">{transaction.service}</p>
                          </div>
                        </td>
                        <td className="px-8 py-6">
                          <div>
                            <p className="font-medium text-slate-900">{transaction.date}</p>
                            {transaction.startTime && transaction.endTime && (
                              <p className="text-slate-600">{transaction.startTime} - {transaction.endTime}</p>
                            )}
                          </div>
                        </td>
                        <td className="px-8 py-6">
                          <div>
                            <p className="font-medium text-slate-900">{transaction.hours}h</p>
                            <p className="text-slate-600">${transaction.rate}/hr</p>
                          </div>
                        </td>
                        <td className="px-8 py-6">
                          <p className="text-xl font-bold text-slate-900">${transaction.total}</p>
                          <p className="text-sm text-slate-500">{transaction.paymentMethod}</p>
                        </td>
                        <td className="px-8 py-6">
                          <span className={`inline-flex items-center px-3 py-1 rounded-full text-sm font-medium ${
                            transaction.status === 'Paid' 
                              ? 'bg-emerald-100 text-emerald-800' 
                              : 'bg-amber-100 text-amber-800'
                          }`}>
                            {transaction.status}
                          </span>
                        </td>
                        <td className="px-8 py-6">
                          <div className="flex items-center gap-2">
                            {transaction.status === 'Pending' && (
                              <button
                                onClick={() => markAsPaid(transaction.id)}
                                className="text-emerald-600 hover:text-emerald-700 p-1 rounded transition-colors"
                                title="Mark as Paid"
                              >
                                <Check className="w-4 h-4" />
                              </button>
                            )}
                            <button
                              onClick={() => deleteTransaction(transaction.id)}
                              className="text-red-600 hover:text-red-700 p-1 rounded transition-colors"
                              title="Delete Transaction"
                            >
                              <Trash2 className="w-4 h-4" />
                            </button>
                          </div>
                        </td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
              
              {filteredTransactions.length === 0 && (
                <div className="text-center py-12">
                  <FileText className="w-12 h-12 text-slate-300 mx-auto mb-4" />
                  <p className="text-slate-500">No transactions found.</p>
                </div>
              )}
            </div>
          </div>
        )}

        {/* Workers Tab */}
        {activeTab === 'workers' && (
          <div className="mt-8 space-y-6">
            <div className="bg-white rounded-2xl shadow-sm border border-slate-200">
              <div className="px-8 py-6 border-b border-slate-200">
                <div className="flex items-center justify-between">
                  <div>
                    <h2 className="text-xl font-bold text-slate-900">Workers</h2>
                    <p className="text-slate-600">Manage your team and contact information</p>
                  </div>
                  <button
                    onClick={() => setShowNewWorkerForm(true)}
                    className="px-4 py-2 bg-emerald-600 text-white rounded-lg hover:bg-emerald-700 transition-colors font-medium flex items-center gap-2"
                  >
                    <Plus className="w-4 h-4" />
                    Add Worker
                  </button>
                </div>
                
                <div className="mt-6">
                  <div className="relative">
                    <Search className="absolute left-4 top-1/2 transform -translate-y-1/2 text-slate-400 w-5 h-5" />
                    <input
                      type="text"
                      placeholder="Search workers..."
                      value={searchTerm}
                      onChange={(e) => setSearchTerm(e.target.value)}
                      className="w-full pl-12 pr-4 py-3 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                    />
                  </div>
                </div>
              </div>

              <div className="p-8">
                <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                  {filteredWorkers.map((worker) => (
                    <div key={worker.id} className="bg-slate-50 p-6 rounded-xl border border-slate-200">
                      <div className="flex items-start justify-between mb-4">
                        <div className="flex items-center gap-3">
                          <div className="w-12 h-12 bg-gradient-to-br from-blue-500 to-indigo-600 rounded-xl flex items-center justify-center">
                            <User className="w-6 h-6 text-white" />
                          </div>
                          <div>
                            <h3 className="text-lg font-semibold text-slate-900">{worker.name}</h3>
                            <p className="text-slate-600">${worker.defaultRate}/hour</p>
                          </div>
                        </div>
                        <div className="flex items-center gap-1">
                          <button
                            onClick={() => {
                              setEditingWorker(worker);
                              setNewWorker({ ...worker });
                              setShowNewWorkerForm(true);
                            }}
                            className="text-blue-600 hover:text-blue-700 p-1 rounded transition-colors"
                            title="Edit Worker"
                          >
                            <Edit2 className="w-4 h-4" />
                          </button>
                          <button
                            onClick={() => deleteWorker(worker.id)}
                            className="text-red-600 hover:text-red-700 p-1 rounded transition-colors"
                            title="Remove Worker"
                          >
                            <Trash2 className="w-4 h-4" />
                          </button>
                        </div>
                      </div>
                      
                      <div className="space-y-3 text-sm">
                        <div className="flex items-center gap-2">
                          <Phone className="w-4 h-4 text-slate-400" />
                          <span className="text-slate-600">{worker.phone}</span>
                        </div>
                        <div className="flex items-center gap-2">
                          <Mail className="w-4 h-4 text-slate-400" />
                          <span className="text-slate-600">{worker.email}</span>
                        </div>
                        {worker.skills && worker.skills.length > 0 && (
                          <div className="pt-2">
                            <div className="flex flex-wrap gap-1">
                              {worker.skills.map((skill) => (
                                <span key={skill} className="px-2 py-1 text-xs bg-blue-100 text-blue-800 rounded-full">
                                  {skill}
                                </span>
                              ))}
                            </div>
                          </div>
                        )}
                      </div>
                    </div>
                  ))}
                </div>

                {filteredWorkers.length === 0 && (
                  <div className="text-center py-12">
                    <User className="w-12 h-12 text-slate-300 mx-auto mb-4" />
                    <p className="text-slate-500">No workers found.</p>
                  </div>
                )}
              </div>
            </div>
          </div>
        )}

        {/* Reports Tab */}
        {activeTab === 'reports' && (
          <div className="mt-8 space-y-8">
            {/* Business Overview */}
            <div className="bg-white rounded-2xl shadow-sm border border-slate-200">
              <div className="px-8 py-6 border-b border-slate-200">
                <h2 className="text-xl font-bold text-slate-900">Business Overview</h2>
                <p className="text-slate-600">Aggregated metrics and performance insights</p>
              </div>
              
              <div className="p-8">
                <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                  {/* Total Business Metrics */}
                  <div className="bg-gradient-to-br from-emerald-50 to-emerald-100 p-6 rounded-xl border border-emerald-200">
                    <h3 className="text-sm font-medium text-emerald-700 mb-2">Total Revenue</h3>
                    <p className="text-3xl font-bold text-emerald-900">
                      ${transactions.filter(t => t.status === 'Paid').reduce((sum, t) => sum + t.total, 0)}
                    </p>
                    <p className="text-sm text-emerald-600 mt-1">
                      {transactions.filter(t => t.status === 'Paid').length} completed sessions
                    </p>
                  </div>

                  <div className="bg-gradient-to-br from-blue-50 to-blue-100 p-6 rounded-xl border border-blue-200">
                    <h3 className="text-sm font-medium text-blue-700 mb-2">Total Hours</h3>
                    <p className="text-3xl font-bold text-blue-900">
                      {transactions.reduce((sum, t) => sum + t.hours, 0)}h
                    </p>
                    <p className="text-sm text-blue-600 mt-1">
                      Avg: {transactions.length > 0 ? (transactions.reduce((sum, t) => sum + t.hours, 0) / transactions.length).toFixed(1) : '0'}h per session
                    </p>
                  </div>

                  <div className="bg-gradient-to-br from-purple-50 to-purple-100 p-6 rounded-xl border border-purple-200">
                    <h3 className="text-sm font-medium text-purple-700 mb-2">Average Rate</h3>
                    <p className="text-3xl font-bold text-purple-900">
                      ${transactions.length > 0 
                        ? (transactions.reduce((sum, t) => sum + t.rate, 0) / transactions.length).toFixed(2)
                        : '0.00'}
                    </p>
                    <p className="text-sm text-purple-600 mt-1">per hour</p>
                  </div>

                  <div className="bg-gradient-to-br from-amber-50 to-amber-100 p-6 rounded-xl border border-amber-200">
                    <h3 className="text-sm font-medium text-amber-700 mb-2">Avg Session Value</h3>
                    <p className="text-3xl font-bold text-amber-900">
                      ${transactions.length > 0 
                        ? (transactions.reduce((sum, t) => sum + t.total, 0) / transactions.length).toFixed(2)
                        : '0.00'}
                    </p>
                    <p className="text-sm text-amber-600 mt-1">per work session</p>
                  </div>
                </div>

                <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
                  {/* Service Breakdown */}
                  <div className="bg-slate-50 p-6 rounded-xl">
                    <h3 className="text-lg font-semibold text-slate-900 mb-4">Service Performance</h3>
                    <div className="space-y-4">
                      {serviceTemplates.map(service => {
                        const serviceTransactions = transactions.filter(t => t.service === service.name);
                        const serviceTotal = serviceTransactions.reduce((sum, t) => sum + t.total, 0);
                        const serviceHours = serviceTransactions.reduce((sum, t) => sum + t.hours, 0);
                        const avgRate = serviceTransactions.length > 0 
                          ? (serviceTransactions.reduce((sum, t) => sum + t.rate, 0) / serviceTransactions.length).toFixed(2)
                          : '0.00';
                        
                        if (serviceTotal > 0) {
                          return (
                            <div key={service.name} className="bg-white p-4 rounded-lg border border-slate-200">
                              <div className="flex justify-between items-start mb-2">
                                <h4 className="font-medium text-slate-900">{service.name}</h4>
                                <span className="text-lg font-bold text-slate-900">${serviceTotal}</span>
                              </div>
                              <div className="grid grid-cols-3 gap-4 text-sm text-slate-600">
                                <div>
                                  <span className="block font-medium">{serviceTransactions.length}</span>
                                  <span>sessions</span>
                                </div>
                                <div>
                                  <span className="block font-medium">{serviceHours}h</span>
                                  <span>total hours</span>
                                </div>
                                <div>
                                  <span className="block font-medium">${avgRate}</span>
                                  <span>avg rate</span>
                                </div>
                              </div>
                            </div>
                          );
                        }
                        return null;
                      })}
                    </div>
                  </div>

                  {/* Payment Methods & Timing */}
                  <div className="bg-slate-50 p-6 rounded-xl">
                    <h3 className="text-lg font-semibold text-slate-900 mb-4">Payment Insights</h3>
                    
                    {/* Payment Methods */}
                    <div className="mb-6">
                      <h4 className="font-medium text-slate-700 mb-3">Payment Methods Used</h4>
                      <div className="space-y-2">
                        {paymentMethods.map(method => {
                          const methodTransactions = transactions.filter(t => t.paymentMethod === method);
                          const methodTotal = methodTransactions.reduce((sum, t) => sum + t.total, 0);
                          
                          if (methodTotal > 0) {
                            return (
                              <div key={method} className="flex justify-between items-center">
                                <span className="text-slate-600">{method}:</span>
                                <div className="text-right">
                                  <span className="font-medium">${methodTotal}</span>
                                  <span className="text-sm text-slate-500 ml-2">({methodTransactions.length})</span>
                                </div>
                              </div>
                            );
                          }
                          return null;
                        })}
                      </div>
                    </div>

                    {/* Monthly Trend */}
                    <div>
                      <h4 className="font-medium text-slate-700 mb-3">This Month vs Last Month</h4>
                      <div className="space-y-2">
                        {(() => {
                          const now = new Date();
                          const thisMonth = transactions.filter(t => {
                            const tDate = new Date(t.date);
                            return tDate.getMonth() === now.getMonth() && tDate.getFullYear() === now.getFullYear();
                          });
                          const lastMonth = transactions.filter(t => {
                            const tDate = new Date(t.date);
                            const lastMonthDate = new Date(now.getFullYear(), now.getMonth() - 1);
                            return tDate.getMonth() === lastMonthDate.getMonth() && tDate.getFullYear() === lastMonthDate.getFullYear();
                          });

                          const thisMonthTotal = thisMonth.reduce((sum, t) => sum + t.total, 0);
                          const lastMonthTotal = lastMonth.reduce((sum, t) => sum + t.total, 0);
                          
                          return (
                            <>
                              <div className="flex justify-between">
                                <span className="text-slate-600">This Month:</span>
                                <span className="font-medium">${thisMonthTotal} ({thisMonth.length} sessions)</span>
                              </div>
                              <div className="flex justify-between">
                                <span className="text-slate-600">Last Month:</span>
                                <span className="font-medium">${lastMonthTotal} ({lastMonth.length} sessions)</span>
                              </div>
                              {lastMonthTotal > 0 && (
                                <div className="flex justify-between pt-2 border-t">
                                  <span className="text-slate-600">Change:</span>
                                  <span className={`font-medium ${
                                    thisMonthTotal >= lastMonthTotal ? 'text-emerald-600' : 'text-red-600'
                                  }`}>
                                    {thisMonthTotal >= lastMonthTotal ? '+' : ''}
                                    ${(thisMonthTotal - lastMonthTotal).toFixed(2)}
                                  </span>
                                </div>
                              )}
                            </>
                          );
                        })()}
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            {/* Worker Performance */}
            <div className="bg-white rounded-2xl shadow-sm border border-slate-200">
              <div className="px-8 py-6 border-b border-slate-200">
                <h2 className="text-xl font-bold text-slate-900">Worker Insights</h2>
                <p className="text-slate-600">Individual performance and activity metrics</p>
              </div>
              
              <div className="p-8">
                <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
                  {workers.map(worker => {
                    const workerTransactions = transactions.filter(t => t.workerId === worker.id);
                    const workerPaidTransactions = workerTransactions.filter(t => t.status === 'Paid');
                    const workerPendingTransactions = workerTransactions.filter(t => t.status === 'Pending');
                    
                    const totalEarned = workerPaidTransactions.reduce((sum, t) => sum + t.total, 0);
                    const totalPending = workerPendingTransactions.reduce((sum, t) => sum + t.total, 0);
                    const totalHours = workerTransactions.reduce((sum, t) => sum + t.hours, 0);
                    const avgRate = workerTransactions.length > 0 
                      ? (workerTransactions.reduce((sum, t) => sum + t.rate, 0) / workerTransactions.length).toFixed(2)
                      : worker.defaultRate.toFixed(2);
                    
                    // Most recent work
                    const recentWork = workerTransactions.sort((a, b) => new Date(b.date) - new Date(a.date))[0];
                    
                    // Service breakdown for this worker
                    const workerServices = {};
                    workerTransactions.forEach(t => {
                      if (!workerServices[t.service]) {
                        workerServices[t.service] = { count: 0, total: 0, hours: 0 };
                      }
                      workerServices[t.service].count++;
                      workerServices[t.service].total += t.total;
                      workerServices[t.service].hours += t.hours;
                    });

                    return (
                      <div key={worker.id} className="bg-slate-50 p-6 rounded-xl">
                        <div className="flex items-center gap-4 mb-6">
                          <div className="w-12 h-12 bg-gradient-to-br from-blue-500 to-indigo-600 rounded-xl flex items-center justify-center">
                            <User className="w-6 h-6 text-white" />
                          </div>
                          <div>
                            <h3 className="text-lg font-semibold text-slate-900">{worker.name}</h3>
                            <p className="text-slate-600">${worker.defaultRate}/hr default rate</p>
                          </div>
                        </div>

                        {/* Key Metrics */}
                        <div className="grid grid-cols-2 gap-4 mb-6">
                          <div className="bg-white p-4 rounded-lg">
                            <h4 className="text-sm font-medium text-slate-600 mb-1">Total Earned</h4>
                            <p className="text-2xl font-bold text-emerald-600">${totalEarned}</p>
                            {totalPending > 0 && (
                              <p className="text-sm text-amber-600">+${totalPending} pending</p>
                            )}
                          </div>
                          <div className="bg-white p-4 rounded-lg">
                            <h4 className="text-sm font-medium text-slate-600 mb-1">Hours Worked</h4>
                            <p className="text-2xl font-bold text-blue-600">{totalHours}h</p>
                            <p className="text-sm text-slate-500">{workerTransactions.length} sessions</p>
                          </div>
                          <div className="bg-white p-4 rounded-lg">
                            <h4 className="text-sm font-medium text-slate-600 mb-1">Average Rate</h4>
                            <p className="text-2xl font-bold text-purple-600">${avgRate}</p>
                            <p className="text-sm text-slate-500">per hour</p>
                          </div>
                          <div className="bg-white p-4 rounded-lg">
                            <h4 className="text-sm font-medium text-slate-600 mb-1">Last Worked</h4>
                            <p className="text-lg font-bold text-slate-900">
                              {recentWork ? recentWork.date : 'Never'}
                            </p>
                            {recentWork && (
                              <p className="text-sm text-slate-500">{recentWork.service}</p>
                            )}
                          </div>
                        </div>

                        {/* Service Breakdown */}
                        {Object.keys(workerServices).length > 0 && (
                          <div>
                            <h4 className="font-medium text-slate-700 mb-3">Services Performed</h4>
                            <div className="space-y-2">
                              {Object.entries(workerServices).map(([service, data]) => (
                                <div key={service} className="bg-white p-3 rounded-lg flex justify-between items-center">
                                  <div>
                                    <span className="font-medium text-slate-900">{service}</span>
                                    <span className="text-sm text-slate-500 ml-2">
                                      {data.count} session{data.count !== 1 ? 's' : ''} • {data.hours}h
                                    </span>
                                  </div>
                                  <span className="font-bold text-slate-900">${data.total}</span>
                                </div>
                              ))}
                            </div>
                          </div>
                        )}

                        {workerTransactions.length === 0 && (
                          <div className="text-center py-6">
                            <p className="text-slate-500">No work sessions recorded yet</p>
                          </div>
                        )}
                      </div>
                    );
                  })}
                </div>

                {workers.length === 0 && (
                  <div className="text-center py-12">
                    <User className="w-12 h-12 text-slate-300 mx-auto mb-4" />
                    <p className="text-slate-500">No workers added yet</p>
                  </div>
                )}
              </div>
            </div>
          </div>
        )}
      </div>

      {/* New Worker Modal */}
      {showNewWorkerForm && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50">
          <div className="bg-white rounded-2xl p-8 w-full max-w-md max-h-[90vh] overflow-y-auto">
            <h3 className="text-xl font-bold text-slate-900 mb-6">
              {editingWorker ? 'Edit Worker' : 'Add New Worker'}
            </h3>
            <div className="space-y-5">
              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Full Name</label>
                <input
                  type="text"
                  placeholder="Enter full name"
                  value={newWorker.name}
                  onChange={(e) => setNewWorker({...newWorker, name: e.target.value})}
                  className={`w-full px-4 py-3 border rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 ${
                    errors.name ? 'border-red-300' : 'border-slate-200'
                  }`}
                />
                {errors.name && <p className="text-red-500 text-sm mt-1">{errors.name}</p>}
              </div>

              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Phone Number</label>
                <input
                  type="text"
                  placeholder="(555) 123-4567"
                  value={newWorker.phone}
                  onChange={(e) => setNewWorker({...newWorker, phone: e.target.value})}
                  className={`w-full px-4 py-3 border rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 ${
                    errors.phone ? 'border-red-300' : 'border-slate-200'
                  }`}
                />
                {errors.phone && <p className="text-red-500 text-sm mt-1">{errors.phone}</p>}
              </div>

              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Email Address</label>
                <input
                  type="email"
                  placeholder="worker@email.com"
                  value={newWorker.email}
                  onChange={(e) => setNewWorker({...newWorker, email: e.target.value})}
                  className={`w-full px-4 py-3 border rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 ${
                    errors.email ? 'border-red-300' : 'border-slate-200'
                  }`}
                />
                {errors.email && <p className="text-red-500 text-sm mt-1">{errors.email}</p>}
              </div>

              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Default Hourly Rate</label>
                <div className="relative">
                  <span className="absolute left-4 top-1/2 transform -translate-y-1/2 text-slate-500">$</span>
                  <input
                    type="number"
                    step="0.50"
                    placeholder="25.00"
                    value={newWorker.defaultRate}
                    onChange={(e) => setNewWorker({...newWorker, defaultRate: parseFloat(e.target.value) || 0})}
                    className={`w-full pl-10 pr-4 py-3 border rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 ${
                      errors.defaultRate ? 'border-red-300' : 'border-slate-200'
                    }`}
                  />
                </div>
                {errors.defaultRate && <p className="text-red-500 text-sm mt-1">{errors.defaultRate}</p>}
              </div>

              <div>
                <label className="block text-sm font-medium text-slate-700 mb-3">Skills (Optional)</label>
                <div className="grid grid-cols-2 gap-3">
                  {['Cleaning', 'Delivery', 'Maintenance', 'Organization', 'Administrative', 'Other'].map(skill => (
                    <label key={skill} className="flex items-center gap-2 text-sm">
                      <input
                        type="checkbox"
                        checked={newWorker.skills?.includes(skill) || false}
                        onChange={(e) => {
                          const skills = newWorker.skills || [];
                          if (e.target.checked) {
                            setNewWorker({...newWorker, skills: [...skills, skill]});
                          } else {
                            setNewWorker({...newWorker, skills: skills.filter(s => s !== skill)});
                          }
                        }}
                        className="rounded text-blue-600 focus:ring-blue-500"
                      />
                      {skill}
                    </label>
                  ))}
                </div>
              </div>
            </div>
            
            <div className="flex gap-3 mt-8">
              <button
                onClick={editingWorker ? updateWorker : addWorker}
                className="flex-1 bg-emerald-600 text-white px-6 py-3 rounded-xl hover:bg-emerald-700 font-medium transition-colors"
              >
                {editingWorker ? 'Update Worker' : 'Add Worker'}
              </button>
              <button
                onClick={() => {
                  setShowNewWorkerForm(false);
                  setEditingWorker(null);
                  setNewWorker({ name: '', phone: '', email: '', defaultRate: 0, skills: [] });
                  setErrors({});
                }}
                className="flex-1 bg-slate-200 text-slate-700 px-6 py-3 rounded-xl hover:bg-slate-300 font-medium transition-colors"
              >
                Cancel
              </button>
            </div>
          </div>
        </div>
      )}

      {/* New Transaction Modal */}
      {showNewTransactionForm && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50">
          <div className="bg-white rounded-2xl p-8 w-full max-w-lg max-h-[90vh] overflow-y-auto">
            <h3 className="text-xl font-bold text-slate-900 mb-6">Record Work Session</h3>
            <div className="space-y-5">
              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Select Worker</label>
                <select
                  value={newTransaction.workerId}
                  onChange={(e) => handleWorkerSelect(e.target.value)}
                  className={`w-full px-4 py-3 border rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 ${
                    errors.workerId ? 'border-red-300' : 'border-slate-200'
                  }`}
                >
                  <option value="">Choose a worker...</option>
                  {workers.filter(w => w.status === 'Active').map(worker => (
                    <option key={worker.id} value={worker.id}>
                      {worker.name} (${worker.defaultRate}/hr)
                    </option>
                  ))}
                </select>
                {errors.workerId && <p className="text-red-500 text-sm mt-1">{errors.workerId}</p>}
              </div>
              
              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Service Type</label>
                <select
                  value={newTransaction.service}
                  onChange={(e) => handleServiceSelect(e.target.value)}
                  className={`w-full px-4 py-3 border rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 ${
                    errors.service ? 'border-red-300' : 'border-slate-200'
                  }`}
                >
                  <option value="">Select service type...</option>
                  {serviceTemplates.map(service => (
                    <option key={service.name} value={service.name}>
                      {service.name}
                    </option>
                  ))}
                </select>
                {errors.service && <p className="text-red-500 text-sm mt-1">{errors.service}</p>}
              </div>

              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Work Date</label>
                <input
                  type="date"
                  value={newTransaction.date}
                  onChange={(e) => setNewTransaction({...newTransaction, date: e.target.value})}
                  className={`w-full px-4 py-3 border rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 ${
                    errors.date ? 'border-red-300' : 'border-slate-200'
                  }`}
                />
                {errors.date && <p className="text-red-500 text-sm mt-1">{errors.date}</p>}
              </div>

              <div className="grid grid-cols-2 gap-4">
                <div>
                  <label className="block text-sm font-medium text-slate-700 mb-2">Start Time</label>
                  <input
                    type="time"
                    value={newTransaction.startTime}
                    onChange={(e) => handleTimeChange('startTime', e.target.value)}
                    className="w-full px-4 py-3 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500"
                  />
                </div>
                <div>
                  <label className="block text-sm font-medium text-slate-700 mb-2">End Time</label>
                  <input
                    type="time"
                    value={newTransaction.endTime}
                    onChange={(e) => handleTimeChange('endTime', e.target.value)}
                    className="w-full px-4 py-3 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500"
                  />
                </div>
              </div>

              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Hours Worked</label>
                <input
                  type="number"
                  step="0.25"
                  placeholder="3.5"
                  value={newTransaction.hours}
                  onChange={(e) => setNewTransaction({...newTransaction, hours: parseFloat(e.target.value) || 0})}
                  className={`w-full px-4 py-3 border rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 ${
                    errors.hours ? 'border-red-300' : 'border-slate-200'
                  }`}
                />
                {errors.hours && <p className="text-red-500 text-sm mt-1">{errors.hours}</p>}
              </div>

              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Hourly Rate</label>
                <div className="relative">
                  <span className="absolute left-4 top-1/2 transform -translate-y-1/2 text-slate-500">$</span>
                  <input
                    type="number"
                    step="0.50"
                    placeholder="25.00"
                    value={newTransaction.rate}
                    onChange={(e) => setNewTransaction({...newTransaction, rate: parseFloat(e.target.value) || 0})}
                    className={`w-full pl-10 pr-4 py-3 border rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 ${
                      errors.rate ? 'border-red-300' : 'border-slate-200'
                    }`}
                  />
                </div>
                {errors.rate && <p className="text-red-500 text-sm mt-1">{errors.rate}</p>}
              </div>

              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Payment Method</label>
                <select
                  value={newTransaction.paymentMethod}
                  onChange={(e) => setNewTransaction({...newTransaction, paymentMethod: e.target.value})}
                  className="w-full px-4 py-3 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500"
                >
                  {paymentMethods.map(method => (
                    <option key={method} value={method}>{method}</option>
                  ))}
                </select>
              </div>

              <div>
                <label className="block text-sm font-medium text-slate-700 mb-2">Notes (Optional)</label>
                <textarea
                  placeholder="Additional details about the work session..."
                  value={newTransaction.notes}
                  onChange={(e) => setNewTransaction({...newTransaction, notes: e.target.value})}
                  className="w-full px-4 py-3 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500"
                  rows="3"
                />
              </div>

              {newTransaction.hours > 0 && newTransaction.rate > 0 && (
                <div className="bg-blue-50 border border-blue-200 p-4 rounded-xl">
                  <div className="flex justify-between items-center">
                    <span className="font-medium text-slate-900">Total Payment:</span>
                    <span className="text-2xl font-bold text-blue-600">
                      ${(newTransaction.hours * newTransaction.rate).toFixed(2)}
                    </span>
                  </div>
                  <p className="text-sm text-blue-600 mt-1">
                    {newTransaction.hours} hours × ${newTransaction.rate}/hour
                  </p>
                </div>
              )}
            </div>
            
            <div className="flex gap-3 mt-8">
              <button
                onClick={addTransaction}
                className="flex-1 bg-blue-600 text-white px-6 py-3 rounded-xl hover:bg-blue-700 font-medium transition-colors"
              >
                Record Session
              </button>
              <button
                onClick={() => {
                  setShowNewTransactionForm(false);
                  setNewTransaction({
                    workerId: '',
                    service: '',
                    date: new Date().toISOString().split('T')[0],
                    startTime: '',
                    endTime: '',
                    hours: 0,
                    rate: 0,
                    notes: '',
                    paymentMethod: 'Cash'
                  });
                  setErrors({});
                }}
                className="flex-1 bg-slate-200 text-slate-700 px-6 py-3 rounded-xl hover:bg-slate-300 font-medium transition-colors"
              >
                Cancel
              </button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default PaymentSystem;
