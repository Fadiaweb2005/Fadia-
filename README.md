   {/* Header */}
        <div className="mb-8 flex flex-col md:flex-row md:items-center justify-between gap-4">
          <div>
            <h1 className="text-3xl font-bold text-slate-800 flex items-center gap-2">
              <BookOpen className="text-blue-600" size={32} />
              ระบบสรุปหนังสือเรียน
            </h1>
            <p className="text-slate-500 mt-1">ติดตามสถานะหนังสือเรียนแยกตามวิชาและระดับชั้น</p>
          </div>
          <div className="bg-white p-1 rounded-xl shadow-sm border border-slate-200 flex items-center px-3">
            <Search size={18} className="text-slate-400" />
            <input 
              type="text" 
              placeholder="ค้นหาวิชาหรือห้องเรียน..." 
              className="bg-transparent border-none focus:ring-0 text-sm p-2 w-full md:w-64"
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
            />
          </div>
        </div>

        {/* Stats Grid */}
        <div className="grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
          <div className="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 flex items-center gap-4">
            <div className="p-3 bg-blue-50 text-blue-600 rounded-xl">
              <Book size={24} />
            </div>
            <div>
              <p className="text-sm text-slate-500">หนังสือทั้งหมด</p>
              <p className="text-2xl font-bold">{stats.total.toLocaleString()}</p>
            </div>
          </div>
          <div className="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 flex items-center gap-4">
            <div className="p-3 bg-emerald-50 text-emerald-600 rounded-xl">
              <CheckCircle2 size={24} />
            </div>
            <div>
              <p className="text-sm text-slate-500">ได้รับแล้ว</p>
              <p className="text-2xl font-bold">{stats.received.toLocaleString()}</p>
            </div>
          </div>
          <div className="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 flex items-center gap-4">
            <div className="p-3 bg-rose-50 text-rose-600 rounded-xl">
              <AlertCircle size={24} />
            </div>
            <div>
              <p className="text-sm text-slate-500">ยังขาดอยู่</p>
              <p className="text-2xl font-bold text-rose-600">{stats.missing.toLocaleString()}</p>
            </div>
          </div>
          <div className="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 flex items-center gap-4">
            <div className="p-3 bg-amber-50 text-amber-600 rounded-xl">
              <Users size={24} />
            </div>
            <div>
              <p className="text-sm text-slate-500">ห้องที่ครบแล้ว</p>
              <p className="text-2xl font-bold">{stats.completeCount} / {stats.totalEntries}</p>
            </div>
          </div>
        </div>

        {/* Filters */}
        <div className="bg-white p-4 rounded-2xl shadow-sm border border-slate-100 mb-6 flex flex-wrap items-center gap-6">
          <div className="flex items-center gap-2">
            <Filter size={18} className="text-slate-400" />
            <span className="text-sm font-medium">กรองข้อมูล:</span>
          </div>
          
          <div className="flex items-center gap-2">
            <label className="text-xs text-slate-500 uppercase tracking-wider font-bold">วิชา</label>
            <select 
              className="bg-slate-50 border-slate-200 rounded-lg text-sm p-2 focus:ring-blue-500 outline-none"
              value={subjectFilter}
              onChange={(e) => setSubjectFilter(e.target.value)}
            >
              {subjects.map(sub => <option key={sub} value={sub}>{sub === 'All' ? 'ทุกวิชา' : sub}</option>)}
            </select>
          </div>

          <div className="flex items-center gap-2">
            <label className="text-xs text-slate-500 uppercase tracking-wider font-bold">ชั้นเรียน</label>
            <select 
              className="bg-slate-50 border-slate-200 rounded-lg text-sm p-2 focus:ring-blue-500 outline-none"
              value={gradeFilter}
              onChange={(e) => setGradeFilter(e.target.value)}
            >
              {grades.map(g => <option key={g} value={g}>{g === 'All' ? 'ทุกชั้น' : g}</option>)}
            </select>
          </div>

          <button 
            onClick={() => {setSubjectFilter('All'); setGradeFilter('All'); setSearchTerm('')}}
            className="text-sm text-blue-600 hover:underline ml-auto"
          >
            ล้างค่าฟิลเตอร์
          </button>
        </div>

        {/* Data Table */}
        <div className="bg-white rounded-2xl shadow-sm border border-slate-100 overflow-hidden">
          <div className="overflow-x-auto">
            <table className="w-full text-left">
              <thead className="bg-slate-50 border-b border-slate-100">
                <tr>
                  <th className="px-6 py-4 text-xs font-bold text-slate-500 uppercase">วิชา</th>
                  <th className="px-6 py-4 text-xs font-bold text-slate-500 uppercase">ชั้น/ห้อง</th>
                  <th className="px-6 py-4 text-xs font-bold text-slate-500 uppercase text-center">เป้าหมาย</th>
                  <th className="px-6 py-4 text-xs font-bold text-slate-500 uppercase text-center">ที่ได้รับ</th>
                  <th className="px-6 py-4 text-xs font-bold text-slate-500 uppercase">ความก้าวหน้า</th>
                  <th className="px-6 py-4 text-xs font-bold text-slate-500 uppercase">สถานะ</th>
                </tr>
              </thead>
              <tbody className="divide-y divide-slate-50">
                {filteredData.length > 0 ? (
                  filteredData.map((item, index) => {
                    const percent = Math.round((item.received / item.total) * 100);
                    const isComplete = item.total === item.received;
                    
                    return (
                      <tr key={index} className="hover:bg-slate-50/50 transition-colors">
                        <td className="px-6 py-4 font-medium">{item.subject}</td>
                        <td className="px-6 py-4">
                          <span className="px-2 py-1 bg-slate-100 rounded text-xs text-slate-600">ป.{item.class}</span>
                        </td>
                        <td className="px-6 py-4 text-center">{item.total}</td>
                        <td className="px-6 py-4 text-center">{item.received}</td>
                        <td className="px-6 py-4 w-48">
                          <div className="flex items-center gap-2">
                            <div className="flex-1 bg-slate-100 h-2 rounded-full overflow-hidden">
                              <div 
                                className={`h-full rounded-full transition-all duration-500 ${isComplete ? 'bg-emerald-500' : 'bg-blue-500'}`} 
                                style={{ width: `${percent}%` }}
                              ></div>
                            </div>
                            <span className="text-xs font-bold w-8">{percent}%</span>
                          </div>
                        </td>
                        <td className="px-6 py-4">
                          <span className={`inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium ${
                            isComplete 
                              ? 'bg-emerald-100 text-emerald-800' 
                              : 'bg-rose-100 text-rose-800'
                          }`}>
                            {item.status}
                          </span>
                        </td>
                      </tr>
                    );
                  })
                ) : (
                  <tr>
                    <td colSpan="6" className="px-6 py-12 text-center text-slate-400">
                      ไม่พบข้อมูลที่ค้นหา
                    </td>
                  </tr>
                )}
              </tbody>
            </table>
          </div>
        </div>
        
        {/* Footer info */}
        <div className="mt-6 text-center text-slate-400 text-xs uppercase tracking-widest">
          Book Tracking System &bull; Updated real-time from CSV
        </div>
      </div>
    </div>
  );
};

export default App;
