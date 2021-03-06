=======================================
Query expansion and Key word extraction
=======================================

Overview
========

Whoosh provides methods for computing the "key terms" of a set of documents. For these methods, "key terms" basically means terms that are frequent in the given documents, but relatively infrequent in the indexed collection as a whole.

Because this is a purely statistical operation, not a natural language processing or AI function, the quality of the results will vary based on the content, the size of the document collection, and the number of documents for which you extract keywords.

These methods can be useful for providing the following features to users:

* Search term expansion. You can extract key terms for the top N results from a query and suggest them to the user as additional/alternate query terms to try.

* Tag suggestion. Extracting the key terms for a single document may yield useful suggestions for tagging the document.

* "More like this". You can extract key terms for the top ten or so results from a query (and removing the original query terms), and use those key words as the basis for another query that may find more documents using terms the user didn't think of.


Usage
=====

* Extract keywords for an arbitrary set of documents.

  Use the :meth:`~whoosh.searching.Searcher.document_number` or :meth:`~whoosh.searching.Searcher.document_number` methods of the :class:`whoosh.searching.Searcher` object to get the document numbers for the document(s) you want to extract keywords from.
    
  Use the :meth:`~whoosh.searching.Searcher.key_terms` method of :class:`whoosh.searching.Searcher` to extract the keywords, given the list of document numbers.
    
  For example, let's say you have an index of emails. To extract key terms from the ``content`` field of emails whose ``emailto`` field contains ``matt@whoosh.ca``::
    
        searcher = email_index.searcher()
        docnums = searcher.document_numbers(emailto=u"matt@whoosh.ca")
        keywords = list(searcher.key_terms(docnums, "content"))

* Extract keywords for the top N documents in a :class:`whoosh.searching.Results` object.

  Use the :meth:`~whoosh.searching.Results.key_terms` method of the :class:`whoosh.searching.Results` object to extract keywords from the top N documents of the result set.
    
  For example, to extract *five* key terms from the ``content`` field of the top *ten* documents of a results object::
    
        keywords = list(results.key_terms("content", docs=10, numterms=5))
        

Expansion models
================

The ``ExpansionModel`` subclasses in the :mod:`whoosh.classify` module implement different weighting functions for key words. These models are translated into Python from original Java implementations in Terrier.
    

