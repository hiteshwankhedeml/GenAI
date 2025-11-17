# BenchMarking

* We created a small evaluation set by asking the business to give some documents/text to be translated
* We assigned a few interns to translate it using google
* We ran the same sentences through IndicTrans, mBART-50, M2M-100 and OPUS-MT.
* We compared them using BLEU
* Indic trans got BLEU of about 28 to 30
* It also gave faster inference, so we selected it for Phase 1.
