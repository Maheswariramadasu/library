/* #1- How many copies of the book titled "The Lost Tribe" are owned by the library branch whose name is "Sharpstown"? */

---->
alter procedure p1
 as
  begin
select c.book_copies_No_Of_Copies from tbl_book b inner join tbl_book_copies c on b.book_BookID=c.book_copies_BookID
inner join tbl_library_branch lb on lb.library_branch_BranchID=c.book_copies_BranchID
where book_Title='The Lost Tribe' and  library_branch_BranchName='Sharpstown'
end

exec p1

/* #2- How many copies of the book titled "The Lost Tribe" are owned by each library branch? */
----->
alter procedure p1
 as
  begin
select lb.library_branch_BranchName,c.book_copies_No_Of_Copies as counting from tbl_book b inner join tbl_book_copies c on b.book_BookID=c.book_copies_BookID
inner join tbl_library_branch lb on lb.library_branch_BranchID=c.book_copies_BranchID
where book_Title='The Lost Tribe'  group by lb.library_branch_BranchName,c.book_copies_No_Of_Copies
end
exec p1


/* #3- Retrieve the names of all borrowers who do not have any books checked out. */
		
----->
alter procedure p1
 as
  begin
select b.borrower_BorrowerName from tbl_borrower b  left outer join tbl_book_loans l on b.borrower_CardNo=l.book_loans_CardNo
		 where  l.book_loans_CardNo is  null 
end

exec p1
/* #4- For each book that is loaned out from the "Sharpstown" branch and whose DueDate is today, retrieve the book title, the borrower's name, and the borrower's address.  */
----->
create procedure p1
 as
  begin
select b.library_branch_BranchName,tbl_book.book_Title,bo.borrower_BorrowerName,bo.borrower_BorrowerAddress,l.book_loans_DueDate from tbl_library_branch b inner join tbl_book_loans l on b.library_branch_BranchID=l.book_loans_BranchID inner join
tbl_borrower bo on bo.borrower_CardNo=l.book_loans_CardNo inner join tbl_book on  tbl_book.book_BookID=l.book_loans_BookID
where b.library_branch_BranchName='Sharpstown' and l.book_loans_DueDate=getdate()
end
/* #5- For each library branch, retrieve the branch name and the total number of books loaned out from that branch.  */
---->
create procedure p1
 as
  begin
select lb.library_branch_BranchName,count(*) as count_branch from tbl_book b inner join tbl_book_loans l on b.book_BookID=l.book_loans_BookID
inner join tbl_library_branch lb on lb.library_branch_BranchID=l.book_loans_BranchID group by lb.library_branch_BranchName
end
/* #6- Retrieve the names, addresses, and number of books checked out for all borrowers who have more than five books checked out. */
create procedure p1
 as
  begin
select b.borrower_BorrowerName,b.borrower_BorrowerAddress,count(bo.book_BookID)  from tbl_borrower b  inner join tbl_book_loans l on b.borrower_CardNo=l.book_loans_CardNo
		inner join tbl_book bo on bo.book_BookID=l.book_loans_BookID group by  b.borrower_BorrowerName,b.borrower_BorrowerAddress
		having count(bo.book_BookID)>5
end
/* #7- For each book authored by "Stephen King", retrieve the title and the number of copies owned by the library branch whose name is "Central".*/
create procedure p1
 as
  begin
 select b.book_Title,count(*),lb.library_branch_BranchName  from tbl_book b  inner join  tbl_book_authors a
 on b.book_BookID=a.book_authors_BookID inner join tbl_book_copies c on c.book_copies_BookID=b.book_BookID inner join tbl_library_branch lb on 
 lb.library_branch_BranchID=c.book_copies_BranchID
  where a.book_authors_AuthorName='Stephen King' and lb.library_branch_BranchName='central'
  group by b.book_Title,lb.library_branch_BranchName
  end

