- 👋 Hi, I’m @hadody2025
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
hadody2025/hadody2025 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import React from 'react';
import { Download } from 'lucide-react';
import { Employee } from '../../types/employee';
import { exportToPDF, exportToCSV } from '../../utils/export';

interface SalaryReportProps {
  employees: Employee[];
  month: string;
}

export default function SalaryReport({ employees, month }: SalaryReportProps) {
  const activeEmployees = employees.filter(emp => emp.paymentStatus === 'active');

  const handleExport = (type: 'pdf' | 'csv') => {
    if (type === 'pdf') {
      exportToPDF(activeEmployees, month);
    } else {
      exportToCSV(activeEmployees, month);
    }
  };

  const calculateTotalEntitlements = (employee: Employee): number => {
    const basicSalary = employee.baseSalary + employee.livingAllowance;
    const allowances = [
      employee.workNatureAllowance,
      employee.practicalWorkAllowance,
      employee.executiveWorkAllowance,
      employee.graduateStudiesAllowance,
      employee.researchBooksAllowance,
      employee.marriageAllowance,
      employee.childrenAllowance,
      employee.infectionAllowance,
      employee.hospitalAllowance,
      employee.administrativeBurdenAllowance,
      employee.carRentAllowance,
      employee.housingAllowance,
      employee.phoneAllowance,
      employee.foundersAdminAllowance,
      employee.experienceYearsAllowance,
      employee.mealAllowance,
      employee.trafficAllowance,
      employee.personalAllowance,
      employee.academicQualificationAllowance,
      employee.disparityRemovalAllowance
    ];
    return basicSalary + allowances.reduce((sum, allowance) => sum + (allowance || 0), 0);
  };

  const calculateTotalDeductions = (employee: Employee): number => {
    const deductions = [
      employee.socialInsurance,
      employee.stamp,
      employee.healthInsurance,
      employee.personalIncomeTax,
      employee.namedTrusts,
      employee.miscTrusts,
      employee.advanceSalary,
      employee.carLoan,
      employee.tuitionLoan,
      employee.housingLoan,
      employee.namedCovenants,
      employee.miscCovenants,
      employee.namedTrustsCovenants,
      employee.emergencyLoan1,
      employee.emergencyLoan2
    ];
    return deductions.reduce((sum, deduction) => sum + (deduction || 0), 0);
  };

  return (
    <div className="bg-white rounded-lg shadow-md p-6 mt-8">
      <div className="flex justify-between items-center mb-6">
        <div>
          <h2 className="text-2xl font-bold text-gray-900">كشف مرتبات</h2>
          <p className="text-gray-600">شهر {month}</p>
        </div>
        <div className="flex gap-2">
          <button
            onClick={() => handleExport('pdf')}
            className="flex items-center px-4 py-2 bg-red-600 text-white rounded hover:bg-red-700"
          >
            <Download className="w-4 h-4 ml-2" />
            PDF حفظ
          </button>
          <button
            onClick={() => handleExport('csv')}
            className="flex items-center px-4 py-2 bg-green-600 text-white rounded hover:bg-green-700"
          >
            <Download className="w-4 h-4 ml-2" />
            CSV حفظ
          </button>
        </div>
      </div>

      <div className="overflow-x-auto">
        <table className="w-full text-right">
          <thead className="bg-gray-50">
            <tr>
              <th className="px-4 py-3 border-b text-sm font-semibold text-gray-700">م</th>
              <th className="px-4 py-3 border-b text-sm font-semibold text-gray-700">رقم الحساب</th>
              <th className="px-4 py-3 border-b text-sm font-semibold text-gray-700">الاسم</th>
              <th className="px-4 py-3 border-b text-sm font-semibold text-gray-700">إجمالي الاستحقاقات</th>
              <th className="px-4 py-3 border-b text-sm font-semibold text-gray-700">إجمالي الاستقطاعات</th>
              <th className="px-4 py-3 border-b text-sm font-semibold text-gray-700">الصافي</th>
            </tr>
          </thead>
          <tbody>
            {activeEmployees.map((employee, index) => {
              const totalEntitlements = calculateTotalEntitlements(employee);
              const totalDeductions = calculateTotalDeductions(employee);
              const netSalary = totalEntitlements - totalDeductions;

              return (
                <tr key={employee.id} className="hover:bg-gray-50">
                  <td className="px-4 py-3 border-b text-sm text-gray-700">{index + 1}</td>
                  <td className="px-4 py-3 border-b text-sm text-gray-700">{employee.bankAccount}</td>
                  <td className="px-4 py-3 border-b text-sm text-gray-700">{employee.name}</td>
                  <td className="px-4 py-3 border-b text-sm text-gray-700">
                    {totalEntitlements.toLocaleString('ar-EG')}
                  </td>
                  <td className="px-4 py-3 border-b text-sm text-gray-700">
                    {totalDeductions.toLocaleString('ar-EG')}
                  </td>
                  <td className="px-4 py-3 border-b text-sm font-semibold text-gray-900">
                    {netSalary.toLocaleString('ar-EG')}
                  </td>
                </tr>
              );
            })}
          </tbody>
        </table>
      </div>
    </div>
  );
}
