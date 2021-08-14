Imports[¶](#Imports) {#Imports}
====================

In [1]:

    import fastbook
    fastbook.setup_book()

In [2]:

    from mnist import MNIST

In [3]:

    import pandas as pd
    import numpy as np

    # import PIL to display images and to create images from arrays
    from PIL import Image

Loading The Training DATA[¶](#Loading-The-Training-DATA) {#Loading-The-Training-DATA}
========================================================

In [7]:

    mndata=MNIST('./data')

In [8]:

    mndata.load_training();

In [9]:

    type(mndata.train_images)

Out[9]:

    list

visualizing & setting the data[¶](#visualizing-&-setting-the-data) {#visualizing-&-setting-the-data}
==================================================================

In [10]:

    df1 = pd.DataFrame (mndata.train_images)

In [11]:

    df1.head(2)

Out[11]:

0

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

56

57

58

59

60

61

62

63

64

65

66

67

68

69

70

71

72

73

74

75

76

77

78

79

80

81

82

83

84

85

86

87

88

89

90

91

92

93

94

95

96

97

98

99

100

101

102

103

104

105

106

107

108

109

110

111

112

113

114

115

116

117

118

119

120

121

122

123

124

125

126

127

128

129

130

131

132

133

134

135

136

137

138

139

140

141

142

143

144

145

146

147

148

149

150

151

152

153

154

155

156

157

158

159

160

161

162

163

164

165

166

167

168

169

170

171

172

173

174

175

176

177

178

179

180

181

182

183

184

185

186

187

188

189

190

191

192

193

194

195

196

197

198

199

200

201

202

203

204

205

206

207

208

209

210

211

212

213

214

215

216

217

218

219

220

221

222

223

224

225

226

227

228

229

230

231

232

233

234

235

236

237

238

239

240

241

242

243

244

245

246

247

248

249

250

251

252

253

254

255

256

257

258

259

260

261

262

263

264

265

266

267

268

269

270

271

272

273

274

275

276

277

278

279

280

281

282

283

284

285

286

287

288

289

290

291

292

293

294

295

296

297

298

299

300

301

302

303

304

305

306

307

308

309

310

311

312

313

314

315

316

317

318

319

320

321

322

323

324

325

326

327

328

329

330

331

332

333

334

335

336

337

338

339

340

341

342

343

344

345

346

347

348

349

350

351

352

353

354

355

356

357

358

359

360

361

362

363

364

365

366

367

368

369

370

371

372

373

374

375

376

377

378

379

380

381

382

383

384

385

386

387

388

389

390

391

392

393

394

395

396

397

398

399

400

401

402

403

404

405

406

407

408

409

410

411

412

413

414

415

416

417

418

419

420

421

422

423

424

425

426

427

428

429

430

431

432

433

434

435

436

437

438

439

440

441

442

443

444

445

446

447

448

449

450

451

452

453

454

455

456

457

458

459

460

461

462

463

464

465

466

467

468

469

470

471

472

473

474

475

476

477

478

479

480

481

482

483

484

485

486

487

488

489

490

491

492

493

494

495

496

497

498

499

500

501

502

503

504

505

506

507

508

509

510

511

512

513

514

515

516

517

518

519

520

521

522

523

524

525

526

527

528

529

530

531

532

533

534

535

536

537

538

539

540

541

542

543

544

545

546

547

548

549

550

551

552

553

554

555

556

557

558

559

560

561

562

563

564

565

566

567

568

569

570

571

572

573

574

575

576

577

578

579

580

581

582

583

584

585

586

587

588

589

590

591

592

593

594

595

596

597

598

599

600

601

602

603

604

605

606

607

608

609

610

611

612

613

614

615

616

617

618

619

620

621

622

623

624

625

626

627

628

629

630

631

632

633

634

635

636

637

638

639

640

641

642

643

644

645

646

647

648

649

650

651

652

653

654

655

656

657

658

659

660

661

662

663

664

665

666

667

668

669

670

671

672

673

674

675

676

677

678

679

680

681

682

683

684

685

686

687

688

689

690

691

692

693

694

695

696

697

698

699

700

701

702

703

704

705

706

707

708

709

710

711

712

713

714

715

716

717

718

719

720

721

722

723

724

725

726

727

728

729

730

731

732

733

734

735

736

737

738

739

740

741

742

743

744

745

746

747

748

749

750

751

752

753

754

755

756

757

758

759

760

761

762

763

764

765

766

767

768

769

770

771

772

773

774

775

776

777

778

779

780

781

782

783

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

3

18

18

18

126

136

175

26

166

255

247

127

0

0

0

0

0

0

0

0

0

0

0

0

30

36

94

154

170

253

253

253

253

253

225

172

253

242

195

64

0

0

0

0

0

0

0

0

0

0

0

49

238

253

253

253

253

253

253

253

253

251

93

82

82

56

39

0

0

0

0

0

0

0

0

0

0

0

0

18

219

253

253

253

253

253

198

182

247

241

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

80

156

107

253

253

205

11

0

43

154

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

14

1

154

253

90

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

139

253

190

2

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

11

190

253

70

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

35

241

225

160

108

1

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

81

240

253

253

119

25

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

45

186

253

253

150

27

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

16

93

252

253

187

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

249

253

249

64

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

46

130

183

253

253

207

2

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

39

148

229

253

253

253

250

182

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

24

114

221

253

253

253

253

201

78

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

23

66

213

253

253

253

253

198

81

2

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

18

171

219

253

253

253

253

195

80

9

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

55

172

226

253

253

253

253

244

133

11

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

136

253

253

253

212

135

132

16

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

1

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

51

159

253

159

50

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

48

238

252

252

252

237

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

54

227

253

252

239

233

252

57

6

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

10

60

224

252

253

252

202

84

252

253

122

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

163

252

252

252

253

252

252

96

189

253

167

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

51

238

253

253

190

114

253

228

47

79

255

168

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

48

238

252

252

179

12

75

121

21

0

0

253

243

50

0

0

0

0

0

0

0

0

0

0

0

0

0

38

165

253

233

208

84

0

0

0

0

0

0

253

252

165

0

0

0

0

0

0

0

0

0

0

0

0

7

178

252

240

71

19

28

0

0

0

0

0

0

253

252

195

0

0

0

0

0

0

0

0

0

0

0

0

57

252

252

63

0

0

0

0

0

0

0

0

0

253

252

195

0

0

0

0

0

0

0

0

0

0

0

0

198

253

190

0

0

0

0

0

0

0

0

0

0

255

253

196

0

0

0

0

0

0

0

0

0

0

0

76

246

252

112

0

0

0

0

0

0

0

0

0

0

253

252

148

0

0

0

0

0

0

0

0

0

0

0

85

252

230

25

0

0

0

0

0

0

0

0

7

135

253

186

12

0

0

0

0

0

0

0

0

0

0

0

85

252

223

0

0

0

0

0

0

0

0

7

131

252

225

71

0

0

0

0

0

0

0

0

0

0

0

0

85

252

145

0

0

0

0

0

0

0

48

165

252

173

0

0

0

0

0

0

0

0

0

0

0

0

0

0

86

253

225

0

0

0

0

0

0

114

238

253

162

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

85

252

249

146

48

29

85

178

225

253

223

167

56

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

85

252

252

252

229

215

252

252

252

196

130

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

28

199

252

252

253

252

252

233

145

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

25

128

252

253

252

141

37

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

In [12]:

    df2 = pd.DataFrame (mndata.train_labels,columns=["label"])

In [13]:

    df2.head(5)

Out[13]:

label

0

5

1

0

2

4

3

1

4

9

In [14]:

    df=pd.concat([df2,df1],axis=1)

In [15]:

    df.head(5)

Out[15]:

label

0

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

56

57

58

59

60

61

62

63

64

65

66

67

68

69

70

71

72

73

74

75

76

77

78

79

80

81

82

83

84

85

86

87

88

89

90

91

92

93

94

95

96

97

98

99

100

101

102

103

104

105

106

107

108

109

110

111

112

113

114

115

116

117

118

119

120

121

122

123

124

125

126

127

128

129

130

131

132

133

134

135

136

137

138

139

140

141

142

143

144

145

146

147

148

149

150

151

152

153

154

155

156

157

158

159

160

161

162

163

164

165

166

167

168

169

170

171

172

173

174

175

176

177

178

179

180

181

182

183

184

185

186

187

188

189

190

191

192

193

194

195

196

197

198

199

200

201

202

203

204

205

206

207

208

209

210

211

212

213

214

215

216

217

218

219

220

221

222

223

224

225

226

227

228

229

230

231

232

233

234

235

236

237

238

239

240

241

242

243

244

245

246

247

248

249

250

251

252

253

254

255

256

257

258

259

260

261

262

263

264

265

266

267

268

269

270

271

272

273

274

275

276

277

278

279

280

281

282

283

284

285

286

287

288

289

290

291

292

293

294

295

296

297

298

299

300

301

302

303

304

305

306

307

308

309

310

311

312

313

314

315

316

317

318

319

320

321

322

323

324

325

326

327

328

329

330

331

332

333

334

335

336

337

338

339

340

341

342

343

344

345

346

347

348

349

350

351

352

353

354

355

356

357

358

359

360

361

362

363

364

365

366

367

368

369

370

371

372

373

374

375

376

377

378

379

380

381

382

383

384

385

386

387

388

389

390

391

392

393

394

395

396

397

398

399

400

401

402

403

404

405

406

407

408

409

410

411

412

413

414

415

416

417

418

419

420

421

422

423

424

425

426

427

428

429

430

431

432

433

434

435

436

437

438

439

440

441

442

443

444

445

446

447

448

449

450

451

452

453

454

455

456

457

458

459

460

461

462

463

464

465

466

467

468

469

470

471

472

473

474

475

476

477

478

479

480

481

482

483

484

485

486

487

488

489

490

491

492

493

494

495

496

497

498

499

500

501

502

503

504

505

506

507

508

509

510

511

512

513

514

515

516

517

518

519

520

521

522

523

524

525

526

527

528

529

530

531

532

533

534

535

536

537

538

539

540

541

542

543

544

545

546

547

548

549

550

551

552

553

554

555

556

557

558

559

560

561

562

563

564

565

566

567

568

569

570

571

572

573

574

575

576

577

578

579

580

581

582

583

584

585

586

587

588

589

590

591

592

593

594

595

596

597

598

599

600

601

602

603

604

605

606

607

608

609

610

611

612

613

614

615

616

617

618

619

620

621

622

623

624

625

626

627

628

629

630

631

632

633

634

635

636

637

638

639

640

641

642

643

644

645

646

647

648

649

650

651

652

653

654

655

656

657

658

659

660

661

662

663

664

665

666

667

668

669

670

671

672

673

674

675

676

677

678

679

680

681

682

683

684

685

686

687

688

689

690

691

692

693

694

695

696

697

698

699

700

701

702

703

704

705

706

707

708

709

710

711

712

713

714

715

716

717

718

719

720

721

722

723

724

725

726

727

728

729

730

731

732

733

734

735

736

737

738

739

740

741

742

743

744

745

746

747

748

749

750

751

752

753

754

755

756

757

758

759

760

761

762

763

764

765

766

767

768

769

770

771

772

773

774

775

776

777

778

779

780

781

782

783

0

5

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

3

18

18

18

126

136

175

26

166

255

247

127

0

0

0

0

0

0

0

0

0

0

0

0

30

36

94

154

170

253

253

253

253

253

225

172

253

242

195

64

0

0

0

0

0

0

0

0

0

0

0

49

238

253

253

253

253

253

253

253

253

251

93

82

82

56

39

0

0

0

0

0

0

0

0

0

0

0

0

18

219

253

253

253

253

253

198

182

247

241

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

80

156

107

253

253

205

11

0

43

154

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

14

1

154

253

90

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

139

253

190

2

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

11

190

253

70

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

35

241

225

160

108

1

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

81

240

253

253

119

25

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

45

186

253

253

150

27

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

16

93

252

253

187

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

249

253

249

64

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

46

130

183

253

253

207

2

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

39

148

229

253

253

253

250

182

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

24

114

221

253

253

253

253

201

78

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

23

66

213

253

253

253

253

198

81

2

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

18

171

219

253

253

253

253

195

80

9

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

55

172

226

253

253

253

253

244

133

11

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

136

253

253

253

212

135

132

16

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

1

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

51

159

253

159

50

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

48

238

252

252

252

237

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

54

227

253

252

239

233

252

57

6

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

10

60

224

252

253

252

202

84

252

253

122

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

163

252

252

252

253

252

252

96

189

253

167

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

51

238

253

253

190

114

253

228

47

79

255

168

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

48

238

252

252

179

12

75

121

21

0

0

253

243

50

0

0

0

0

0

0

0

0

0

0

0

0

0

38

165

253

233

208

84

0

0

0

0

0

0

253

252

165

0

0

0

0

0

0

0

0

0

0

0

0

7

178

252

240

71

19

28

0

0

0

0

0

0

253

252

195

0

0

0

0

0

0

0

0

0

0

0

0

57

252

252

63

0

0

0

0

0

0

0

0

0

253

252

195

0

0

0

0

0

0

0

0

0

0

0

0

198

253

190

0

0

0

0

0

0

0

0

0

0

255

253

196

0

0

0

0

0

0

0

0

0

0

0

76

246

252

112

0

0

0

0

0

0

0

0

0

0

253

252

148

0

0

0

0

0

0

0

0

0

0

0

85

252

230

25

0

0

0

0

0

0

0

0

7

135

253

186

12

0

0

0

0

0

0

0

0

0

0

0

85

252

223

0

0

0

0

0

0

0

0

7

131

252

225

71

0

0

0

0

0

0

0

0

0

0

0

0

85

252

145

0

0

0

0

0

0

0

48

165

252

173

0

0

0

0

0

0

0

0

0

0

0

0

0

0

86

253

225

0

0

0

0

0

0

114

238

253

162

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

85

252

249

146

48

29

85

178

225

253

223

167

56

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

85

252

252

252

229

215

252

252

252

196

130

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

28

199

252

252

253

252

252

233

145

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

25

128

252

253

252

141

37

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

2

4

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

67

232

39

0

0

0

0

0

0

0

0

0

62

81

0

0

0

0

0

0

0

0

0

0

0

0

0

0

120

180

39

0

0

0

0

0

0

0

0

0

126

163

0

0

0

0

0

0

0

0

0

0

0

0

0

2

153

210

40

0

0

0

0

0

0

0

0

0

220

163

0

0

0

0

0

0

0

0

0

0

0

0

0

27

254

162

0

0

0

0

0

0

0

0

0

0

222

163

0

0

0

0

0

0

0

0

0

0

0

0

0

183

254

125

0

0

0

0

0

0

0

0

0

46

245

163

0

0

0

0

0

0

0

0

0

0

0

0

0

198

254

56

0

0

0

0

0

0

0

0

0

120

254

163

0

0

0

0

0

0

0

0

0

0

0

0

23

231

254

29

0

0

0

0

0

0

0

0

0

159

254

120

0

0

0

0

0

0

0

0

0

0

0

0

163

254

216

16

0

0

0

0

0

0

0

0

0

159

254

67

0

0

0

0

0

0

0

0

0

14

86

178

248

254

91

0

0

0

0

0

0

0

0

0

0

159

254

85

0

0

0

47

49

116

144

150

241

243

234

179

241

252

40

0

0

0

0

0

0

0

0

0

0

150

253

237

207

207

207

253

254

250

240

198

143

91

28

5

233

250

0

0

0

0

0

0

0

0

0

0

0

0

119

177

177

177

177

177

98

56

0

0

0

0

0

102

254

220

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

169

254

137

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

169

254

57

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

169

254

57

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

169

255

94

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

169

254

96

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

169

254

153

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

169

255

153

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

96

254

153

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

3

1

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

124

253

255

63

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

96

244

251

253

62

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

127

251

251

253

62

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

68

236

251

211

31

8

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

60

228

251

251

94

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

155

253

253

189

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

20

253

251

235

66

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

32

205

253

251

126

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

104

251

253

184

15

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

80

240

251

193

23

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

32

253

253

253

159

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

151

251

251

251

39

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

48

221

251

251

172

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

234

251

251

196

12

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

253

251

251

89

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

159

255

253

253

31

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

48

228

253

247

140

8

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

64

251

253

220

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

64

251

253

220

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

24

193

253

220

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

4

9

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

55

148

210

253

253

113

87

148

55

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

87

232

252

253

189

210

252

252

253

168

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

4

57

242

252

190

65

5

12

182

252

253

116

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

96

252

252

183

14

0

0

92

252

252

225

21

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

132

253

252

146

14

0

0

0

215

252

252

79

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

126

253

247

176

9

0

0

8

78

245

253

129

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

16

232

252

176

0

0

0

36

201

252

252

169

11

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

22

252

252

30

22

119

197

241

253

252

251

77

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

16

231

252

253

252

252

252

226

227

252

231

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

55

235

253

217

138

42

24

192

252

143

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

62

255

253

109

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

71

253

252

21

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

253

252

21

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

71

253

252

21

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

106

253

252

21

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

45

255

253

21

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

218

252

56

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

96

252

189

42

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

14

184

252

170

11

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

14

147

252

42

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

0

In [16]:

    def saveDigit(digit, filepath):
        digit = digit.reshape(28,28)
        digit = digit.astype(np.uint8)

        img = Image.fromarray(digit)
        img.save(filepath)

Saving the data like in the fastai tutorial[¶](#Saving-the-data-like-in-the-fastai-tutorial) {#Saving-the-data-like-in-the-fastai-tutorial}
============================================================================================

In [19]:

    from fastbook import *
    TRAIN = Path("./train")

    for index in range(10):
        try:
            os.makedirs(TRAIN/str(index))
        except:
            pass

In [20]:

    sorted(os.listdir(TRAIN))

Out[20]:

    ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']

In [45]:

    for index, row in df.iterrows():
        
        label,digit = row["label"], row[1:]
        
        folder = TRAIN/str(label)
        filename = f"{index}.jpg"
        filepath = folder/filename
        
        digit = digit.values
        
        saveDigit(digit, filepath)

In [22]:

    numbers = '0','1','2','3','4','5','6','7','8','9'

In [23]:

    fns = get_image_files(TRAIN)
    fns

Out[23]:

    (#60000) [Path('train/0/11304.jpg'),Path('train/0/59363.jpg'),Path('train/0/4201.jpg'),Path('train/0/23154.jpg'),Path('train/0/38161.jpg'),Path('train/0/33485.jpg'),Path('train/0/6737.jpg'),Path('train/0/37790.jpg'),Path('train/0/44335.jpg'),Path('train/0/9478.jpg')...]

Creating The Datablock and setting the model[¶](#Creating-The-Datablock-and-setting-the-model) {#Creating-The-Datablock-and-setting-the-model}
==============================================================================================

In [24]:

    numbers = DataBlock(
        blocks=(ImageBlock, CategoryBlock), 
        get_items=get_image_files, 
        splitter=RandomSplitter(valid_pct=0.2, seed=42),
        get_y=parent_label,
        item_tfms=Resize(32))

In [25]:

        dls = numbers.dataloaders(TRAIN)

In [26]:

    dls.valid.show_batch(max_n=4, nrows=1)

![](data:image/png;base64,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%0A)

In [31]:

    #defining the model
    learn = cnn_learner(dls, resnet18, metrics=accuracy)

    learn.model = learn.model.cuda()

    #fitting the model
    learn.fine_tune(4);

  epoch   train\_loss   valid\_loss   accuracy   time
  ------- ------------- ------------- ---------- -------
  0       0.722394      0.489605      0.843500   00:30

  epoch   train\_loss   valid\_loss   accuracy   time
  ------- ------------- ------------- ---------- -------
  0       0.144452      0.080211      0.976917   00:35
  1       0.084866      0.051654      0.984167   00:34
  2       0.042311      0.034666      0.989750   00:34
  3       0.017383      0.033313      0.990417   00:35

In [33]:

    #Exporting the learner
    learn.export('./Mnist.pkl')

In [28]:

    #Importing the learner
    learn= load_learner("./Mnist.pkl")

In [32]:

    #plotting the confusion matrics
    interp = ClassificationInterpretation.from_learner(learn)
    interp.plot_confusion_matrix(figsize=(7,7))

![](data:image/png;base64,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%0A)

In [34]:

    #Plotting the top Losses
    interp.plot_top_losses(3, nrows=3)

![](data:image/png;base64,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%0A)

In [35]:

    #Testing my image
    uploader = widgets.FileUpload()
    uploader

In [44]:

    learn.predict(PILImage.create(uploader.data[0]))[0]

Out[44]:

    '8'
