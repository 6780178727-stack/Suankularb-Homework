<main className="container main">
    {/* Tabs */}
    <div className="tab-list">
      <button
        className={`tab ${tab === "teacher" ? "tab-active" : ""}`}
        onClick={() => setTab("teacher")}
      >
        สำหรับครู
      </button>
      <button
        className={`tab ${tab === "student" ? "tab-active" : ""}`}
        onClick={() => setTab("student")}
      >
        สำหรับนักเรียน
      </button>
      <button
        className={`tab ${tab === "admin" ? "tab-active" : ""}`}
        onClick={() => setTab("admin")}
      >
        แดชบอร์ดผู้บริหาร
      </button>
    </div>

    {/* TEACHER TAB */}
    {tab === "teacher" && (
      <>
        <section className="card">
          <h2 className="card-title">มอบหมายการบ้าน</h2>
          <p className="card-description">ระบุรายวิชา ระดับชั้น และกำหนดส่ง</p>

          <div className="grid-3 mt-12">
            <div>
              <label className="label">รายวิชา</label>
              <select
                className="select"
                value={subject}
                onChange={(e) => setSubject(e.target.value)}
              >
                {SUBJECTS.map((s) => (
                  <option key={s} value={s}>
                    {s}
                  </option>
                ))}
              </select>
            </div>
            <div>
              <label className="label">ระดับชั้น</label>
              <select
                className="select"
                value={classLevel}
                onChange={(e) => setClassLevel(e.target.value)}
              >
                {CLASS_LEVELS.map((c) => (
                  <option key={c} value={c}>
                    {c}
                  </option>
                ))}
              </select>
            </div>
            <div>
              <label className="label">หัวข้อการบ้าน</label>
              <input
                className="input"
                value={title}
                onChange={(e) => setTitle(e.target.value)}
                placeholder="เช่น แบบฝึกหัดหน้าที่ 45 ข้อ 1–10"
              />
            </div>
          </div>

          <div className="grid-3 mt-12">
            <div>
              <label className="label">วันที่มอบ</label>
              <input
                type="date"
                className="input"
                value={assignedDate}
                onChange={(e) => setAssignedDate(e.target.value)}
              />
            </div>
            <div>
              <label className="label">กำหนดส่ง</label>
              <input
                type="date"
                className="input"
                value={dueDate}
                onChange={(e) => setDueDate(e.target.value)}
              />
            </div>
            <div>
              <label className="label">แนบลิงก์</label>
              <input
                className="input"
                value={link}
                onChange={(e) => setLink(e.target.value)}
                placeholder="Google Doc / Classroom / YouTube"
              />
            </div>
          </div>

          <div className="mt-12">
            <label className="label">คำอธิบาย</label>
            <textarea
              className="textarea"
              value={description}
              onChange={(e) => setDescription(e.target.value)}
              placeholder="รายละเอียด / เกณฑ์การให้คะแนน"
            />
          </div>

          <div className="mt-12">
            <span className="label">แม่แบบด่วน</span>
            <div>
              {QUICK_TEMPLATES.map((q, idx) => (
                <button
                  key={idx}
                  className="btn btn-sm"
                  style={{ marginRight: 6, marginTop: 4 }}
                  type="button"
                  onClick={() => {
                    setTitle(q.title);
                    if (q.description) setDescription(q.description);
                  }}
                >
                  ➕ {q.title}
                </button>
              ))}
            </div>
          </div>

          <div className="row-between mt-12">
            <p className="helper">ระบบจะบันทึกข้อมูลอัตโนมัติในอุปกรณ์นี้ (ต้นแบบ)</p>
            <button className="btn btn-primary" onClick={addHomework}>
              เพิ่มการบ้าน
            </button>
          </div>
        </section>

        <section className="card">
          <div className="row-between">
            <div>
              <h2 className="card-title">รายการการบ้านทั้งหมด</h2>
              <p className="card-description">จัดเรียง/ค้นหาตามตัวกรองด้านขวา</p>
            </div>
            <div className="chip-filter-group">
              <span className="chip">ตัวกรอง</span>
              <select
                className="select"
                style={{ maxWidth: 150 }}
                value={tSubject}
                onChange={(e) => setTSubject(e.target.value as any)}
              >
                <option value="ALL">ทุกวิชา</option>
                {SUBJECTS.map((s) => (
                  <option key={s} value={s}>
                    {s}
                  </option>
                ))}
              </select>
              <select
                className="select"
                style={{ maxWidth: 120 }}
                value={tClass}
                onChange={(e) => setTClass(e.target.value as any)}
              >
                <option value="ALL">ทุกชั้น</option>
                {CLASS_LEVELS.map((c) => (
                  <option key={c} value={c}>
                    {c}
                  </option>
                ))}
              </select>
              <input
                className="input"
                style={{ minWidth: 180 }}
                placeholder="ค้นหา..."
                value={tSearch}
                onChange={(e) => setTSearch(e.target.value)}
              />
            </div>
          </div>

          <div className="mt-12" style={{ overflowX: "auto" }}>
            <table className="table">
              <thead>
                <tr>
                  <th>หัวข้อ</th>
                  <th>วิชา</th>
                  <th>ชั้น</th>
                  <th>มอบ</th>
                  <th>กำหนดส่ง</th>
                  <th className="text-right">จัดการ</th>
                </tr>
              </thead>
              <tbody>
                {teacherList.length === 0 && (
                  <tr>
                    <td colSpan={6} className="text-center">
                      ยังไม่มีการบ้าน หรือเงื่อนไขตัวกรองไม่ตรงกับข้อมูล
                    </td>
                  </tr>
                )}
                {teacherList.map((i) => {
                  const days = daysUntil(i.dueDate);
                  const badge = dueLabel(days);
                  return (
                    <tr key={i.id}>
                      <td>
                        <div>{i.title}</div>
                        {i.description && (
                          <div className="helper">{i.description}</div>
                        )}
                        {i.link && (
                          <div>
                            <a
                              href={i.link}
                              target="_blank"
                              rel="noreferrer"
                              className="helper"
                              style={{ color: "#2563eb" }}
                            >
                              เปิดลิงก์
                            </a>
                          </div>
                        )}
                      </td>
                      <td>{i.subject}</td>
                      <td>{i.classLevel}</td>
                      <td className="helper">{displayDate(i.assignedDate)}</td>
                      <td>
                        <span className={badge.className}>{badge.text}</span>
                      </td>
                      <td className="text-right">
                        <button
                          className="btn btn-ghost btn-sm"
                          onClick={() => removeHomework(i.id)}
                        >
                          ลบ
                        </button>
                      </td>
                    </tr>
                  );
                })}
              </tbody>
            </table>
          </div>
        </section>
      </>
    )}

    {/* STUDENT TAB */}
    {tab === "student" && (
      <section className="card">
        <h2 className="card-title">นักเรียนเช็คการบ้าน</h2>
        <p className="card-description">
          เลือกระดับชั้นและกรอกชื่อนักเรียนเพื่อบันทึกความคืบหน้าเฉพาะบุคคล
        </p>

        <div className="grid-3 mt-12">
          <div>
            <label className="label">ระดับชั้น</label>
            <select
              className="select"
              value={studentClass}
              onChange={(e) => setStudentClass(e.target.value)}
            >
              {CLASS_LEVELS.map((c) => (
                <option key={c} value={c}>
                  {c}
                </option>
              ))}
            </select>
          </div>
          <div style={{ gridColumn: "span 2" }}>
            <label className="label">ชื่อนักเรียน</label>
            <input
              className="input"
              value={studentName}
              onChange={(e) => setStudentName(e.target.value)}
              placeholder="พิมพ์ชื่อ–นามสกุล"
            />
            <p className="helper">
              ระบบจะจดจำสถานะการบ้านตามชื่อที่กรอก (ใช้ชื่อเดิมทุกครั้ง)
            </p>
          </div>
        </div>

        <div className="row-between mt-12">
          <label style={{ display: "flex", alignItems: "center", gap: 6 }}>
            <input
              type="checkbox"
              checked={showCompleted}
              onChange={(e) => setShowCompleted(e.target.checked)}
            />
            <span style={{ fontSize: 13 }}>แสดงรายการที่ทำแล้ว</span>
          </label>
          <div style={{ fontSize: 13, color: "#4b5563" }}>
            ทั้งหมด {filteredItems.length} งาน • แสดง {visibleItems.length}
          </div>
        </div>

        <div className="mt-12">
          {visibleItems.length === 0 && (
            <div className="text-center helper" style={{ padding: "32px 0" }}>
              ยังไม่มีการบ้านสำหรับชั้นนี้ หรือทำครบแล้ว 🎉
            </div>
          )}

          {visibleItems.map((i) => {
            const done = !!studentData[i.id];
            const days = daysUntil(i.dueDate);
            const badge = dueLabel(days);
            return (
              <div
                key={i.id}
                className="card"
                style={{
                  padding: 12,
                  marginBottom: 10,
                  borderRadius: 14
                }}
              >
                <div className="row-between">
                  <div>
                    <div style={{ marginBottom: 4 }}>
                      <span className="badge badge-outline" style={{ marginRight: 6 }}>
                        {i.subject}
                      </span>
                      <span className={badge.className}>{badge.text}</span>
                    </div>
                    <div style={{ fontWeight: 600 }}>{i.title}</div>
                    {i.description && (
                      <div className="helper" style={{ marginTop: 4 }}>
                        {i.description}
                      </div>
                    )}
                    {i.link && (
                      <div style={{ marginTop: 4 }}>
                        <a
                          href={i.link}
                          target="_blank"
                          rel="noreferrer"
                          className="helper"
                          style={{ color: "#2563eb" }}
                        >
                          เปิดลิงก์งาน
                        </a>
                      </div>
                    )}
                    <div className="helper" style={{ marginTop: 6 }}>
                      มอบ {displayDate(i.assignedDate)} • กำหนดส่ง {displayDate(i.dueDate)}
                    </div>
                  </div>

                  <label style={{ display: "flex", alignItems: "center", gap: 6 }}>
                    <input
                      type="checkbox"
                      checked={done}
                      onChange={(e) => toggleDone(studentName, i.id, e.target.checked)}
                    />
                    <span style={{ fontSize: 13 }}>ทำแล้ว</span>
                  </label>
                </div>
              </div>
            );
          })}
        </div>
      </section>
    )}

    {/* ADMIN TAB */}
    {tab === "admin" && (
      <>
        <section className="card">
          <h2 className="card-title">จำนวนการบ้านตามรายวิชา</h2>
          <p className="card-description">
            ใช้ดูภาพรวมภาระงานนักเรียนในแต่ละวิชา (นับตามจำนวนงานที่มอบ)
          </p>
          <div className="mt-12" style={{ overflowX: "auto" }}>
            <table className="table">
              <thead>
                <tr>
                  <th>วิชา</th>
                  <th>จำนวนการบ้าน</th>
                </tr>
              </thead>
              <tbody>
                {subjectSummary.length === 0 && (
                  <tr>
                    <td colSpan={2} className="text-center">
                      ยังไม่มีข้อมูลการบ้าน
                    </td>
                  </tr>
                )}
                {subjectSummary.map((row) => (
                  <tr key={row.subject}>
                    <td>{row.subject}</td>
                    <td>{row.count}</td>
                  </tr>
                ))}
              </tbody>
            </table>
          </div>
        </section>

        <section className="card">
          <h2 className="card-title">สรุปภาพรวมตามระดับชั้น</h2>
          <p className="card-description">
            จำนวนงานที่มอบและจำนวนการติ๊ก “ทำแล้ว” (รวมทุกนักเรียน) เพื่อประเมินแนวโน้มการส่งงาน
          </p>
          <div className="mt-12" style={{ overflowX: "auto" }}>
            <table className="table">
              <thead>
                <tr>
                  <th>ระดับชั้น</th>
                  <th>จำนวนงานทั้งหมด</th>
                  <th>จำนวนติ๊กทำแล้ว (รวม)</th>
                  <th>% การทำงาน (คร่าว ๆ)</th>
                </tr>
              </thead>
              <tbody>
                {classSummary.length === 0 && (
                  <tr>
                    <td colSpan={4} className="text-center">
                      ยังไม่มีข้อมูลเพียงพอ
                    </td>
                  </tr>
                )}
                {classSummary.map((row) => (
                  <tr key={row.classLevel}>
                    <td>{row.classLevel}</td>
                    <td>{row.total}</td>
                    <td>{row.doneCount}</td>
                    <td>{row.percent}%</td>
                  </tr>
                ))}
              </tbody>
            </table>
          </div>
          <p className="helper mt-8">
            * ตัวเลข % เป็นการประเมินจากจำนวนการติ๊กทำแล้ว (ไม่ได้แยกตามรายชื่อจริงในระดับระบบใหญ่)
          </p>
        </section>
      </>
    )}
  </main>

  <footer className="footer">
    © {new Date().getFullYear()} Suankularb Wittayalai • SKHW Homework Tracker • ICT Smart
    School Initiative
  </footer>
</div># Suankularb-Homework
